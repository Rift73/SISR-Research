# Contract: a defensible TensorRT comparison

[Home](../README.md) · [Evidence](../evidence/README.md)

The question is not “what number did a benchmark print?” It is **which decision
does this comparison support?** Most project measurements are seeded-engine
feasibility tests, not trained-quality or application-throughput evaluations.

## Write the contract before optimizing

Record hardware and software versions, batch, LR and output shapes, scale,
geometry, weights/initialization, precision and rounding, TF32, optimization level,
workspace, auxiliary streams, graph capture, warm-up, timed samples and transfer
inclusion. State whether timing measures GPU compute, host latency or the complete
application pipeline. These are not interchangeable.

For memory, distinguish parameters, engine file bytes, execution-context allocation,
allocator allocated/reserved peaks, optimizer state and total process/device VRAM.
Resetting an allocator counter after graph capture can miss memory already owned
by graph pools. A tiny post-reset peak is not proof of tiny training memory.

## Establish a non-vacuous implementation comparison

- Keep a reference for the **candidate's own function**. A narrowed model need
  not equal its parent. Own-function parity does not prove quality retention.
- Check a nonzero calibration state where new branches affect output and receive
  gradients. A zero output projection can hide a broken upstream implementation.
- Preserve masks, bias, scale and intentional cast/round points. Choosing a faster
  attention backend by dropping a bias changes the model.
- Use the best supported native fused backend first. An unsupported contract is a
  reason to choose another valid implementation, not silently change semantics.

## Optimize and measure both sides fairly

1. Give the unchanged baseline comparable export, layout and plugin optimization.
2. Build under the same explicit settings; inspect actual fusion and stream use.
3. Load in a fresh process and check parity/finite output. Preserve meaningful
   build or reload failures even if later validation passes.
4. Alternate complete-engine runs, such as `A/B, B/A, A/B`, with repeatable warm-up.
   Keep every timed sample and report round-level as well as pooled summaries.
5. Use same-engine A/A and independent rebuilds when the claimed delta is small
   enough that tactic selection or run variability could change the decision.

Consecutive timing samples are not independent training seeds. Reporting hundreds
of samples cannot establish a quality confidence interval or remove systematic
measurement bias.

## Profile to explain, not to replace the outcome

Isolated kernels identify candidates for optimization. Their sums may not predict
the complete graph because fusion, layouts, launches and neighboring tactics change.
Instrumented profile totals can differ from ordinary graph timings.

For a fraction `p` sped up by factor `s`, the simple Amdahl estimate is
`1 / ((1-p) + p/s)`. It assumes the remaining workload is unchanged. Do not declare
a hardware lower bound from one profile, or use peak bandwidth as a guaranteed
read-time estimate. New packing/projection costs belong in the candidate total.

## Keep three conclusions separate

| Evidence | Supports | Does not establish |
| --- | --- | --- |
| Reference/output/gradient checks | Tested implementation contracts | Convergence or preferred detail |
| Seeded TensorRT comparison | Cost under the named deployment settings | Trained checkpoint parity or end-to-end app speed |
| Owner's training and visual observations | Value in those reported runs | A component's isolated causal effect |

Only combine these into an adoption claim when the missing links are actually
tested. Until then, publish the useful result and its limits together.
