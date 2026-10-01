# 04 — X, XG and XC: matching is not the same as transporting detail

[Journey](../docs/journey.md) · Evidence: S08 in [provenance](../evidence/provenance.md)

**Status:** X/XG implemented, measured and owner-trained; XC retired for insufficient
practical gain. These are internal-example methods, not external-prior models.

## The missing capability being investigated

A trunk may recognize useful structures without directly bringing a matched
example's detail into the reconstruction. GRAFT-X introduced internal cross-scale
retrieval, with updates to LR features and the 2× reconstruction feature stream.
This uses evidence within the image, not an external pretrained dictionary.
Cross-scale self-exemplars have clear antecedents in
[CS-NL](https://arxiv.org/abs/2006.01424) and [IGNN](https://arxiv.org/abs/2006.16673).

XG then added a second reconstruction-scale route. It reused query/key descriptors
but transported different values derived from **projected HR2 features**, not
predicted RGB pixels. The routes have independent temperatures, so shared Q/K
does not mean identical attention probabilities or a single reused softmax.

Schematic matching is:

$$A_r=\operatorname{softmax}(\tau_r QK^\top s),\qquad O_r=A_rV_r.$$

Changing temperature can alter concentration even when query/key rankings agree.
Both retrievals can send gradients into the shared descriptors. Phase-aware value
packing determines which retrieved feature lands at each output subpixel.

## What made the experiment informative

Zero-initialized added readouts provided a baseline function at initialization.
They also initially block gradients into some upstream new-branch parameters.
An identity test on that state is insufficient evidence that the active branch
is correct. Nonzero calibrated readouts and explicit slot/index checks were needed.

The owner reported that X could improve perceived accuracy and texture with little
metric movement. XG improved aggregate metrics, yet original GRAFT could still be
preferred for small-detail fidelity. The XG-versus-GRAFT report included matched
150k comparisons; the three-way ranking also included X at a different step.
None of this isolates a single operator's causal effect.

XC added richer value/phase mechanisms, but its gains were judged too modest.
Its multiple fixed phase banks are **static layouts**, not data-dependent bank
selection. Static arrangement alone does not guarantee that a compiler removes
packing or materialization costs.

**Reusable lesson:** separately specify (1) what establishes correspondence,
(2) what gets transported, (3) where it lands, and (4) how a bad match is rejected.
More retrieval is not automatically more faithful detail; lower reconstruction
error is not proof that the right fine structure was recovered.
