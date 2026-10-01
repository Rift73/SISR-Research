# 08 — The frontier: preserve useful detail, not just aggregate scores

[Journey](../docs/journey.md) · Evidence: S14–S16 in [provenance](../evidence/provenance.md)

**Status:** window comparison measured; successor architectures below remain
unbuilt hypotheses. Unchanged, optimized DUAL2 is the reference.

## One concrete result: context size is expensive

The later DUAL2 comparison changed both context sizes: **`[32,64] → [48,96]`**.
The seeded engines measured **56.42530 → 86.87865 ms**, or **53.97% more latency**,
with identical folded weights and equivalent graph optimization effort.
All 34 attention cores were fused in both arms.

This is not a trained-quality comparison. Larger support changes the function
even when the checkpoint loads unchanged. Anchored last windows also overlap
when a window does not divide the image. The [geometry contract](../docs/window-geometry.md)
shows why the attention-pair increase is larger than a simple area intuition.

## The remaining design question

QPA proved that substantial execution savings were possible, but did not preserve
the owner's preferred fine detail. DUAL2 suggested another opportunity: improve
training without enlarging deployment. Neither observation tells us which expensive
attention updates are safely dispensable, or whether another transport mechanism
would reconstruct more faithful detail.

The latest research keeps two different directions, not a stack of every idea:

| Direction | Complete idea at the research level | Main objection | What could change the decision? |
| --- | --- | --- | --- |
| **DUAL2-EC: coverage-aware computation** | Retain one or two of four widest-context passes per spatial tile. Start with fixed coverage; consider a learned top-up only if concentration helps | Dropping a pass also removes its local refinement. Coverage does not guarantee equivalent information, and sparse packing can erase savings | A bounded comparison of fixed coverage and concentration that retains fine detail and has a credible complete execution path |
| **G-V / SVT: alternative detail transport** | Retrieve distant, content-related evidence while preserving value identity/phase and allowing rejection of a bad match | Short training crops may not teach full-page retrieval; grouping, projection, abstention and tiled boundaries have real costs | A mechanism test that separates correct detail transport from averaging or confidently copying the wrong pattern |

The first is explicitly incremental in novelty, not a renamed breakthrough. The
second is more ambitious but less closed and potentially costlier. A previous
combined CAT-G proposal was demoted; it is not the current recommendation.

## Research gaps that survived review

1. **An output-error map is not an internal tile-value oracle.** Removing all
   widest-context layers jointly cannot identify each layer/tile's marginal
   value. Interactions, substitutes and complements matter.
2. **A selector must be learnable under the actual crop regime.** Per-window and
   per-image budgets induce different masks. A full-page deployment policy cannot
   be justified by a short-crop training calculation alone.
3. **Phase checks must detect the failure of interest.** A coherence magnitude can
   look perfect under a constant wrong phase. Rejection needs to remain rejection
   after the output projection and its bias.
4. **Count the complete operator.** Query/key/value/output projections, grouping,
   scatters, saved state and backward can rival the advertised attention core.

**Smallest useful next step, not executed:** define the allocation estimand and
border/mask contract, then use a bounded trained-model screen to decide whether
fixed coverage deserves a prototype. Include small-detail inspection, not only
aggregate metrics. Such a screen does not prove retrained quality or eliminate
all possible efficient attention designs if it fails.

GPU experiments are deferred during ongoing training. Missing future evidence is
neither a measured failure nor proof that the architecture space is saturated.
