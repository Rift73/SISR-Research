# 02 — GRAFT: representation and geometry are part of the contract

[Journey](../docs/journey.md) · Evidence: S05 in [provenance](../evidence/provenance.md)

**Status:** v0 was an untrained release; v1 was implemented, selected tiers measured
and Light trained by the owner. Current DUAL inherits v1, not v0.

## The question and design

The research moved beyond coarse conditioning toward a transformer trunk combining
local detail processing with wider context. DRFT/SST experience informed comparisons,
but GRAFT is not documented as a direct WSR code descendant.

The GRAFT v1 Light six-layer group is:

`G16 → shifted G16 → C32 → G16 → shifted G16 → C-large`

Four groups give 24 trunk attention layers. Local G layers retain dense learned
relative-position bias. C layers use signed relative Fourier factors appended to
query/key vectors. This distinction matters: the latter can avoid a dense additive
score-bias tensor in a compatible fused attention contract. The local bias cannot
simply be dropped to obtain the same backend path.

The current inherited trunk uses per-token normalization with FP32 statistics.
Older v0 or experimental image/local-holistic normalization descriptions should
not be pasted into the explanation of current DUAL.

The earlier v0 bundled local-holistic normalization, edge-anchored context and a
different gated feed-forward design. Moving to v1 was not evidence that every v0
idea failed in training: v0 had not been trained. In particular, changing the
statistical support of normalization between a crop and a page was a deployment/
generalization risk, so the simpler per-token contract became the default.

## The consequential distinction

**State compatibility is not function identity.** An earlier stage plan used
`[32,64]` context windows for LR64 pretraining and `[32,48]` for later LR96 stages.
Parameters can load while the attended sets—and therefore computation—change.
That historical plan is not proof of the owner's current deployed configuration.
The later requested `[48,96]` experiment is another distinct geometry.

At a boundary, the actual anchored partition determines which tokens are duplicated,
which are returned, and what positional differences are represented. “Window size
64” without the partition rule, crop size and inference shape is incomplete.

## What the exploration did not establish

A smaller fixed-window, reduced-depth comparison cannot isolate the effect of
normalization or position encoding. Earlier claims of an empirical i-LN benefit
were withdrawn. These are reasons to correct the evidence map—not to ban an entire
normalization family or declare transformers saturated.

**Reusable lesson:** write a train-to-deploy contract for normalization support,
positions, borders, crop sizes, precision and checkpoint loading. Test the actual
contract boundary, not just a divisible square where all implementations coincide.

For the broader architectural context, [SwinIR](https://arxiv.org/abs/2108.10257)
and [HAT](https://arxiv.org/abs/2205.04437) are relevant antecedents. They are not
evidence that this project's particular organization is superior.
