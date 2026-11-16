# Results: 429 vLLM Token-Generation Benchmark

**Main goal:** Measure vLLM token generation rate  
**Tools:** Start python -m vllm.entrypoints.openai.api_server on port 8000, then run scripts/benchmark_serving.py

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_142 · recorded 2026-11-16). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 NVL (95830 MiB) · AMD EPYC 9374F 32-Core Processor · Ubuntu 26.04.1 LTS · kernel 6.8.0-138-generic · driver 595.91.07
- **Run:** 2026-11-16T11:46:34Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 03:05 · benchmark 00:01 · whole run 03:19
- **Source:** run 429_20261116_114634
- **Command:** `bash run_benchmark.sh --smoke --model-name mistralai/Mistral-7B-v0.3 --dtype bfloat16 --tensor-parallel-size 1 --gpu-memory-utilization 0.9 --input-len 64 --output-len 16 --max-num-seqs 2 --max-concurrency 2`

| # | Metric | Result |
|---|---|---|
| 1 | Generated token rate (output_token_throughput_tokens_sec) | 300.1349929253512 |
| 2 | TPOT, ms (time_per_output_token_tpot_p50_msec) | 5.779507880409558 |
| 3 | Time To First Token, ms (ttft_p50_msec) | 12.848095037043095 |
| 4 | ITL, ms (inter_token_latency_itl_p50_msec) | 5.762356799095869 |
| 5 | End-to-end request latency p50, ms (end_to_end_request_latency_msec) | 99.98413035646081 |
<!-- /profile:smoke -->
