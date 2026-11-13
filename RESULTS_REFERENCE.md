# Results: 429 vLLM Token-Generation Benchmark

**Main goal:** Measure vLLM token generation rate  
**Tools:** Start python -m vllm.entrypoints.openai.api_server on port 8000, then run scripts/benchmark_serving.py

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_138 · recorded 2026-11-12). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 80GB HBM3 (81559 MiB) · Intel(R) Xeon(R) Platinum 8470 · Ubuntu 26.04.1 LTS · kernel 6.8.0-147-generic · driver 595.91.07
- **Run:** 2026-11-12T23:52:36Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 03:45 · benchmark 00:30 · whole run 04:32
- **Source:** run 429_20261112_235236
- **Command:** `bash run_benchmark.sh --smoke --model-name mistralai/Mistral-7B-v0.3 --dtype bfloat16 --tensor-parallel-size 1 --gpu-memory-utilization 0.9 --input-len 64 --output-len 16 --max-num-seqs 2 --max-concurrency 2`

| # | Metric | Result |
|---|---|---|
| 1 | Generated token rate (output_token_throughput_tokens_sec) | 271.769937513725 |
| 2 | TPOT, ms (time_per_output_token_tpot_p50_msec) | 6.029608799144626 |
| 3 | Time To First Token, ms (ttft_p50_msec) | 20.120456931181252 |
| 4 | ITL, ms (inter_token_latency_itl_p50_msec) | 6.068464019335806 |
| 5 | End-to-end request latency p50, ms (end_to_end_request_latency_msec) | 110.86673790123314 |
<!-- /profile:smoke -->
