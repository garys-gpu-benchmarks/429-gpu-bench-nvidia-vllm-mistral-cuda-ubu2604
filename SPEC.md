# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Starts python -m vllm.entrypoints.openai.api_server with mistralai/Mistral-7B-v0.3 on 127.0.0.1:8000 for every profile, waits for /v1/models, runs scripts/benchmark_serving.py once with the yaml output length, then stops the server. Measures generated-token rate, TTFT, TPOT, ITL, and end-to-end latency. output_format: csv Sweep dimensions: model_name, dtype, tensor_parallel_size, gpu_memory_utilization, prompt_source, input_len, output_len, max_num_seqs.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| model_name | `--model-name` | smoke=mistralai/Mistral-7B-v0.3, baseline=mistralai/Mistral-7B-v0.3, extended=mistralai/Mistral-7B-v0.3 | mistralai/Mistral-7B-v0.3 | From Parameter list; see Execution Description With Parameters. |
| dtype | `--dtype` | smoke=bfloat16, baseline=bfloat16, extended=bfloat16 | bfloat16 | From Parameter list; see Execution Description With Parameters. |
| tensor_parallel_size | `--tensor-parallel-size` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| gpu_memory_utilization | `--gpu-memory-utilization` | smoke=0.9, baseline=0.9, extended=0.9 | 0.9 | From Parameter list; see Execution Description With Parameters. |
| prompt_source | `--prompt-source` | smoke=synthetic, baseline=synthetic, extended=synthetic | synthetic | From Parameter list; see Execution Description With Parameters. |
| input_len | `--input-len` | smoke=64, baseline=512, extended=1024 | 512 | From Parameter list; see Execution Description With Parameters. |
| output_len | `--output-len` | smoke=16, baseline=18600, extended=52600 | 18600 | From Parameter list; see Execution Description With Parameters. |
| max_num_seqs | `--max-num-seqs` | smoke=2, baseline=8, extended=16 | 8 | From Parameter list; see Execution Description With Parameters. |
| max_concurrency | `--max-concurrency` | smoke=2, baseline=8, extended=8 | 8 | From Parameter list; see Execution Description With Parameters. |
| request_rate | `--request-rate` | smoke=inf, baseline=inf, extended=inf | inf | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Start python -m vllm.entrypoints.openai.api_server on port 8000, then run scripts/benchmark_serving.py
```

## Raw Output Format

requests.csv has one row per request. raw_results.csv has summary and summary_repeat of the same p50 metrics

check_name,status,output_token_throughput_tokens_sec,ttft_p50_msec,ttft_p95_msec,ttft_p99_msec,time_per_output_token_tpot_p50_msec,time_per_output_token_tpot_p95_msec,time_per_output_token_tpot_p99_msec,inter_token_latency_itl_p50_msec,inter_token_latency_itl_p95_msec,inter_token_latency_itl_p99_msec,end_to_end_request_latency_msec
summary,ok,50,30,55,80,18,28,40,17,26,36,400

## Metrics

- **#1: Generated token rate** — stored as `output_token_throughput_tokens_sec`.
- **#2: TPOT, ms** — stored as `time_per_output_token_tpot_p50_msec`.
- **#3: Time To First Token, ms** — stored as `ttft_p50_msec`.
- **#4: ITL, ms** — stored as `inter_token_latency_itl_p50_msec`.
- **#5: Total request completion time** — stored as `end_to_end_request_latency_msec`.

## Framework

Starts python -m vllm.entrypoints.openai.api_server with mistralai/Mistral-7B-v0.3 on 127.0.0.1:8000 for every profile, waits for /v1/models, runs scripts/benchmark_serving.py once with the yaml output length, then stops the server. Measures generated-token rate, TTFT, TPOT, ITL, and end-to-end latency. output_format: csv

## Installation and Execution Summary

Start python -m vllm.entrypoints.openai.api_server --model mistralai/Mistral-7B-v0.3 on 127.0.0.1:8000 for every profile, wait for /v1/models, run scripts/benchmark_serving.py once with the yaml output length, then stop the server, to measure generated-token rate, TTFT, TPOT, ITL, and end-to-end latency

## Platform Portability

- **AMD (primary):** ```bash
Start python -m vllm.entrypoints.openai.api_server on port 8000, then run scripts/benchmark_serving.py
```
- **NVIDIA:** Native NVIDIA CUDA workload. Execute on the stated Ubuntu release with the host NVIDIA driver and CUDA userspace. ROCm porting notes do not apply.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

requests.csv has one row per request. raw_results.csv has summary and summary_repeat of the same p50 metrics

check_name,status,output_token_throughput_tokens_sec,ttft_p50_msec,ttft_p95_msec,ttft_p99_msec,time_per_output_token_tpot_p50_msec,time_per_output_token_tpot_p95_msec,time_per_output_token_tpot_p99_msec,inter_token_latency_itl_p50_msec,inter_token_latency_itl_p95_msec,inter_token_latency_itl_p99_msec,end_to_end_request_latency_msec
summary,ok,50,30,55,80,18,28,40,17,26,36,400

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Starts python -m vllm.entrypoints.openai.api_server with mistralai/Mistral-7B-v0.3 on 127.0.0.1:8000 for every profile, waits for /v1/models, runs scripts/benchmark_serving.py once with the yaml output length, then stops the server. Measures generated-token rate, TTFT, TPOT, ITL, and end-to-end latency. output_format: csv
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Starts python -m vllm.entrypoints.openai.api_server with mistralai/Mistral-7B-v0.3 on 127.0.0.1:8000 for every profile, waits for /v1/models, runs scripts/benchmark_serving.py once with the yaml output length, then stops the server. Measures generated-token rate, TTFT, TPOT, ITL, and end-to-end latency. output_format: csv

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
