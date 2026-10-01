# 06 — QPA: a strong metric/latency result can miss the objective

[Journey](../docs/journey.md) · Evidence: S11–S12 in [provenance](../evidence/provenance.md)

**Status:** implemented, synthetic deployment measured and owner-trained; not the
preferred quality model, but not automatically retired as an efficiency option.

## The proposal and declared tradeoff

DUAL-QPA targeted the four widest-context attention modules. It grouped queries
in 2×2 structures and changed value packing while keeping fused attention feasible.
It also narrowed per-head Q/K from 32 to 8 and V from 32 to 12, with corresponding
gate/projection changes. Distinct value slots remained, but the matching/transport
contract changed. Parameter count fell **3,273,201 → 3,147,729**.

This was an intentional architecture/capacity change—not lossless packing and
not merely a better implementation of DUAL. Permuted attention methods such as
[SRFormer](https://arxiv.org/abs/2303.09735v1) are relevant prior art, not proof
that this particular tradeoff preserves detail.

## What passed

The bounded prototype passed its own reference/gradient/numerical checks and
retained native fused attention. In a fresh, equally optimized paired experiment,
**DUAL 55.16075 ms → QPA 44.65575 ms**, a **19.04% latency reduction** at B1 LR512,
4×, under the TensorRT contract in the [evidence index](../evidence/README.md).

Execution-context memory also decreased slightly. Neither quantity was a full
training-memory result. Synthetic forward/backward checks were separate from
optimizer, data-loader, EMA and complete trainer behavior.

## What changed the adoption decision

The owner later reported strong Urban100 results and slightly lower Manga109
metrics than XG, yet noticeably worse fine-detail fidelity than XG and especially
DUAL: **missing or incorrect fine details**. The shared 53k Manga109 SSIM tooltip
showed rounded DUAL 0.9228, XG 0.9224 and QPA 0.9223.

A small aggregate difference does not bound output differences. Improvements and
regressions can cancel across images, regions or structures. Even for MSE,

$$\operatorname{MSE}(e+d)=\operatorname{MSE}(e)+\operatorname{mean}(d^2)
  +2\operatorname{mean}(ed).$$

Ignoring the cross term cannot reveal the amplitude or sparsity of visual errors.
Nor can those SSIM values identify their spatial pattern. The specific phenotype
comes from the owner's report, not a deduction from the metric.

## What remains unresolved

Shared matching, narrowed representations and their interactions changed jointly.
The later delivered wrapper also globally pads irregular inputs, unlike DUAL's
native handling. This is a genuine possible confound, not a demonstrated bug or
a proven border-localized cause. The regular-shape prototype timing does not
qualify every later irregular deployment shape.

A useful future check would separate regular/native boundaries from wrapper
behavior and test matching versus width changes with appropriate controls. Cropping
alone changes content and context too; it is a screen, not causal isolation.

**Reusable lesson:** QPA advanced a measured resource tradeoff and supplied useful
quality observations. It was not adopted as the preferred quality model. Keep both
facts. A two-axis metric/latency frontier need not be the frontier of the actual
objective, which includes faithful detail.
