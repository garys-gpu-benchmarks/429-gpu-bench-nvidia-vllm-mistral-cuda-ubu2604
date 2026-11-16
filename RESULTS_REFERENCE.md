# Results: 429 vLLM Token-Generation Benchmark

**Main goal:** Measure vLLM token generation rate  
**Tools:** Start python -m vllm.entrypoints.openai.api_server on port 8000, then run scripts/benchmark_serving.py

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_143 · recorded 2026-11-16). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 NVL (95830 MiB) · AMD EPYC 9374F 32-Core Processor · Ubuntu 26.04.1 LTS · kernel 6.8.0-138-generic · driver 595.91.07
- **Run:** 2026-11-16T20:09:29Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 03:06 · benchmark 00:01 · whole run 03:20
- **Source:** run 429_20261116_200929
- **Command:** `bash run_benchmark.sh --smoke --model-name mistralai/Mistral-7B-v0.3 --dtype bfloat16 --tensor-parallel-size 1 --gpu-memory-utilization 0.9 --input-len 64 --output-len 16 --max-num-seqs 2 --max-concurrency 2`

| # | Metric | Result |
|---|---|---|
| 1 | Generated token rate (output_token_throughput_tokens_sec) | 297.5207433479027 |
| 2 | TPOT, ms (time_per_output_token_tpot_p50_msec) | 5.799624199668567 |
| 3 | Time To First Token, ms (ttft_p50_msec) | 13.297292869538069 |
| 4 | ITL, ms (inter_token_latency_itl_p50_msec) | 5.812948103994131 |
| 5 | End-to-end request latency p50, ms (end_to_end_request_latency_msec) | 100.70291208103299 |
<!-- /profile:smoke -->
