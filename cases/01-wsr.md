# 01 — WSR: spend the execution budget where it helps

[Journey](../docs/journey.md) · Evidence: S01–S04 in [provenance](../evidence/provenance.md)

**Status:** implemented and synthetic deployment measured; owner-reported tier
preferences. The family consolidation is not a new quality experiment.

## The question

Could a model do more useful work at roughly SPAN F64's deployment latency,
instead of simply minimizing parameters or convolution MACs?

The starting evidence was an actual engine profile: activation/gating and other
full-resolution memory passes were consequential. Some convolution/activation/
residual combinations fused well; others did not. This suggested a different
allocation of the same latency budget, not a universal claim that convolutions
are free or that every model is bandwidth-bound.

## The synthesis

WideContextSR combined a full-resolution wide residual stream with a cheaper
coarse context stream. The latter conditioned selected fine-stream blocks:

$$y' = y\odot 2\sigma(\operatorname{up}(\gamma)+a\odot y)
       +\operatorname{up}(\beta).$$

The coarse path starts from rearranged input pixels, with a mid-depth fine-to-coarse
merge. Rearrangement preserves samples; subsequent learned channel mappings are
not automatically information-preserving. Zero modulation/readout initialization
makes the context path initially inert, so performance checks must activate it.

This draws on known ideas: wide activation ([WDSR](https://arxiv.org/abs/1808.08718)),
structural reparameterization and gating ([SPAN](https://arxiv.org/abs/2311.12770)),
and conditional feature modulation ([SFT](https://arxiv.org/abs/1804.02815)). WSR's
contribution here is their particular synthesis and measured execution mapping,
not invention of those primitives or use of SFT's semantic-segmentation prior.

## What changed during testing

- Activation and upsampling choices followed measured fusion behavior, not a
  generic preference for a smoother activation or a library-documentation claim.
- Pristine zero-initialized branches were useful for identity checks but inadequate
  for non-vacuous deployment/gradient tests. Seeded nonzero calibration states were used.
- An initial selection used uncontrolled auxiliary streams. After rebuilding the
  grid with explicit single-stream settings, the selected context-only model was
  **D8/S3**, not the earlier D8/S4 favorite.
- Two final builds measured captured latency ratios **1.00394 and 1.00650 versus
  SPAN**, under the historical batch-2 LR540×960 FP16 contract. That established
  a latency-class result, not trained quality superiority.

## Why WESR followed

Context modulation can tell the fine stream what to emphasize without explicitly
transporting a matching example's detail. WESR added block-token exemplar attention.
Fine queries retrieved content from learned 4×4 LR block tokens inside context
windows, making correspondence explicit rather than only modulating a local stream.
The whole-graph cost included downstream layout effects and output rearrangement,
not just the fused attention kernel. A selected D7 configuration retained exemplar
attention; it was not a shared-weight “remove one block” causal ablation.

Later family consolidation followed the owner's reported experience: exemplar
backbones were favored at Default/Compact, while context-only designs remained at
smaller budgets. This was a budget-dependent observation, not a universal CNN-versus-
attention verdict. Naming consolidation did not create a new quality result.

**Reusable lesson:** profile an actual bottleneck, propose a mechanism for using
the recovered budget, and re-measure the complete graph. A locally cheap operator
can still make its neighbors expensive. Keep the baseline equally optimized.
