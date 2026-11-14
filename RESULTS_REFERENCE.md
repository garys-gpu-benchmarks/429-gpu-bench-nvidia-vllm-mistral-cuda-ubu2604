# Results: 429 vLLM Token-Generation Benchmark

**Main goal:** Measure vLLM token generation rate  
**Tools:** Start python -m vllm.entrypoints.openai.api_server on port 8000, then run scripts/benchmark_serving.py

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_140 · recorded 2026-11-13). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 PCIe (81559 MiB) · Intel(R) Xeon(R) Platinum 8352Y CPU @ 2.20GHz · Ubuntu 26.04.1 LTS · kernel 6.8.0-138-generic · driver 595.91.07
- **Run:** 2026-11-13T21:33:55Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 04:28 · benchmark 00:43 · whole run 05:40
- **Source:** run 429_20261113_213355
- **Command:** `bash run_benchmark.sh --smoke --model-name mistralai/Mistral-7B-v0.3 --dtype bfloat16 --tensor-parallel-size 1 --gpu-memory-utilization 0.9 --input-len 64 --output-len 16 --max-num-seqs 2 --max-concurrency 2`

| # | Metric | Result |
|---|---|---|
| 1 | Generated token rate (output_token_throughput_tokens_sec) | 189.94039258037205 |
| 2 | TPOT, ms (time_per_output_token_tpot_p50_msec) | 8.997887993852297 |
| 3 | Time To First Token, ms (ttft_p50_msec) | 21.523843053728342 |
| 4 | ITL, ms (inter_token_latency_itl_p50_msec) | 8.982793893665075 |
| 5 | End-to-end request latency p50, ms (end_to_end_request_latency_msec) | 157.24274888634682 |
<!-- /profile:smoke -->
