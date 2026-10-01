# Evidence index

[Home](../README.md) · [Quality observations](quality-observations.md) · [Status](status-ledger.md) · [Provenance](provenance.md)

**This is not a cross-model leaderboard.** Numbers below are project measurements
under specific conditions. Quality observations are in a separate ledger. None
of these seeded-engine timings establishes trained-checkpoint quality in TensorRT.

## Paired measurements with included samples

Both experiments use an **RTX 5090, TensorRT 11.2, batch 1, RGB LR512×512 →
4× output2048×2048**, static Opt3, strongly typed BF16/FP32, TF32 off, zero
auxiliary streams, an 8 GiB workspace pool and one inference stream. Timings are
**CUDA-graph GPU compute excluding transfers**, not application latency. Seeded,
untrained weights were used. The window record's tool banner is `v110201`.

Each experiment has three interleaved rounds, `A/B, B/A, A/B`, and 102 recorded
compute samples per arm per round: **306 per arm**. Each round used 1000 ms
warm-up, requested 101 iterations and zero minimum timed duration; 102 is the
actual returned row count, not an invented match to the requested count.

| Experiment | A | B | Pooled median A | Pooled median B | Within-experiment change |
| --- | --- | --- | ---: | ---: | ---: |
| QPA feasibility, Sep 30 | Equally optimized unchanged DUAL | QPA prototype | 55.16075 ms | 44.65575 ms | B has 19.04% lower latency |
| DUAL2 windows, Oct 1 | DUAL2 `[32,64]` | DUAL2 `[48,96]` | 56.42530 ms | 86.87865 ms | B has 53.97% higher latency |

| Round | DUAL | QPA | DUAL2 `[32,64]` | DUAL2 `[48,96]` |
| ---: | ---: | ---: | ---: | ---: |
| 1 | 55.21005 | 44.64115 | 56.35170 | 86.77575 |
| 2 | 54.99620 | 44.68225 | 56.44165 | 86.87795 |
| 3 | 55.17065 | 44.67105 | 56.51300 | 87.14040 |

Round values are median milliseconds. The first two columns form one comparison;
the last two form another. **Do not subtract DUAL from DUAL2 across these experiments.**

### What was held—and not held—constant

- **QPA:** equivalent optimization effort, retained fused attention, unchanged
  inherited weights where compatible. The four targeted attention branches change
  their function and width. This is not identical-model execution optimization.
- **Windows:** identical folded DUAL2 weights; only the context pair changes.
  Both arms preserve anchored non-divisible windows and all 34 fused attention
  cores. Different attended sets intentionally give different functions.
- Both were checked against their own numerical reference. Parity is not a quality
  test. In-process post-save reloads emitted duplicate embedded-plugin registration
  errors; fresh-process loads, parity and timings passed. The errors were retained,
  not relabeled as absent.

Execution-context allocations were **854,640,640 → 835,962,880 bytes** for DUAL/QPA
and **854,640,640 → 981,452,800 bytes** for the DUAL2 window pair. These are not
complete process VRAM, peak training memory or optimizer-state measurements.

### Inspect or recompute

[latency.json](latency.json) contains only the contract, round order, per-round
compute samples and recomputed summaries. Flatten an arm's three sample arrays,
sort them, and average the two middle values to reproduce its pooled median.
The median of the three round medians is a different statistic.

Hashes, device identifiers, private paths, raw command logs and host timestamps
were excluded. This lets readers inspect arithmetic and variability, but does
not supply the missing code/weights needed to reproduce engine construction.

## Selected earlier decision-changing measurements

These are recorded same-session pairs or ratios, not newly rerun tests. Raw samples
are not bundled for this historical subset. Sources S02, S07, S09 and S10 describe
their limits in [provenance](provenance.md).

| Evidence | Result | Contract and limitation |
| --- | --- | --- |
| WideContextSR D8/S3 vs SPAN, two builds | Captured ratios 1.00394 and 1.00650 | RTX 5090; batch 2 LR540×960, 4×, FP16, Opt1, auxiliary streams 0; untrained; independent-build latency-class check |
| FOCUS-PK vs XG | 86.1787 vs 50.8093 ms | RTX 5090; batch 1 LR512, 4×, Opt3, strongly typed BF16/FP32, TF32 off, graph compute excluding I/O; seeded optimized graphs |
| DUAL vs XG | 57.6875 vs 52.0522 ms | Same broad B1 LR512/4×/Opt3 BF16/FP32 contract; zero auxiliary streams, 8 GiB workspace; three interleaved rounds, 303 samples per arm |
| Exact DUAL execution pilot | 57.6433 → 55.8740 ms | Same broad B1 LR512/4×/Opt3 BF16/FP32 contract; zero auxiliary streams, 8 GiB workspace; three rounds, 306 samples per arm; FP32 reassociation, not bitwise |

The complete [benchmarking contract](../docs/benchmarking.md) explains why a small
parameter count, isolated kernel win or theoretical operation count is insufficient.
