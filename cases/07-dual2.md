# 07 — DUAL2: change how the convolution learns, not its deployed size

[Journey](../docs/journey.md) · Evidence: S13–S15 in [provenance](../evidence/provenance.md)

**Status:** implemented, synthetic deployment measured, owner-trained; current
preferred quality reference. The quality improvement's cause is unresolved.

## The question

Could the useful training parameterization from the WSR line improve DUAL without
replacing its preferred attention/detail paths or enlarging its deployed kernels?

## What changed

Current DUAL2 replaces eleven dense stride-one 3×3 convolutions with a linear
`1×1 → 3×3 → 1×1` chain plus a 1×1 skip, with no activation inside the chain.
The effective kernel is composed differentiably during training and applied as
one convolution. Export folds an evaluation copy back to ordinary 3×3 convolutions.
The attention architecture stays DUAL's, unlike QPA2.

The source counts are **6,188,150 trainable parameters versus 3,273,201 after
folding**. Extra factors and their optimizer state are a training cost. Avoiding
expanded activation maps does not make the additional parameterization free.
See the [folding contract](../docs/reparameterization.md) for the equation and borders.

This combines a WSR/Conv3XC-style parameterization with differentiable kernel
composition. Those are distinct choices: where parameters live and how forward
execution evaluates them. The latter was also used in prior DRFT work; it does
not require an extra user-facing switch merely because another model has one.

## What was observed

**Reported, not a causal ablation:** early matched-step tooltips showed a tiny
Manga109 SSIM advantage and near-flat/slightly worse Urban100. As training
continued, the owner reported DUAL2 ahead on both. The latest report supplied no
new values or metric breakdown. See the [observation ledger](../evidence/quality-observations.md).

**Measured, not a DUAL-vs-DUAL2 speed claim:** DUAL2's own `[32,64]` engine ran at
56.42530 ms in the later window experiment. DUAL's earlier timings were recorded
separately. They must not be subtracted to infer the cost of reparameterization.

## Wrong turns and remaining explanations

- An earlier proposal folded two linear spatial 3×3 stages to a 5×5 kernel.
  That enlarged the deployed kernel and is **not current DUAL2**. A valid fold of
  a new branch would not make that branch equivalent to the original 3×3 model.
- Matching the initial effective function does not match the training trajectory.
  The optimizer updates factors, not a single kernel. Even a learning-rate control
  is only a partial explanation of the resulting dynamics.
- The source preset uses parameter EMA. Averaging factors and then folding is
  generally different from averaging effective kernels. This is a potential
  explanatory variable, not an established advantage or an EMA bug.
- Run variation and unverified recipe identity remain possible explanations.

**Reusable lesson:** distinguish deployed function family, instantaneous function,
training parameterization and evaluation averaging. A change can preserve the
first two at initialization while changing the latter two throughout training.

A future causal check should separate online weights, factor-space EMA and
effective-kernel EMA, alongside optimization controls. That is an experiment to
justify later—not a reason to alter a promising ongoing run.
