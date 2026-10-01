# 05 — DUAL: complementary interactions, then execution engineering

[Journey](../docs/journey.md) · Evidence: S09–S10 in [provenance](../evidence/provenance.md)

**Status:** implemented, synthetic deployment measured and owner-trained; a strong
quality reference, later superseded in the owner's preference by DUAL2.

## The architectural hypothesis

Spatial attention compares positions. Channel attention estimates interactions
among feature channels. DUAL adds eight regional channel sublayers to XG, after
context spatial residuals and before the feed-forward blocks, retaining the existing
spatial and reconstruction routes.

The regional matrices come from four half-offset families of clipped supports,
blended with weights summing to one at each position. This is a structured way
to avoid a hard change of channel interaction at a region boundary. That algebraic
blending property does not prove it caused the reported quality gain.

This is related to transposed/channel attention in
[Restormer](https://arxiv.org/abs/2111.09881), but the regional supports, blending
and integration into this trunk are specific design choices. It is architectural
synthesis, not invention of channel attention.

The hypothesis was that these complementary interactions could help restoration
without sacrificing the useful spatial path. The parameter count nevertheless
increased **10.73%** over XG. Reported quality gains cannot be attributed solely
to the mechanism while ignoring that capacity difference.

## Why implementation changed the practical verdict

The initial explicit regional graph was expensive. Successive execution work
addressed tensor layout, regional Gram/statistic construction, application and
projection-add. The architecture was not narrowed to obtain these savings.

The retained early paired result was **57.6875 ms DUAL versus 52.0522 ms XG**.
A later exact-execution pilot measured **55.8740 versus 57.6433 ms** for its selected
and unchanged DUAL graphs in a new matched session. These are separate experiments,
not values to mix into a single cross-session ranking.

“Exact” here denotes the intended operation contract, not bitwise arithmetic.
Reassociation of FP32 reductions and specified BF16 round points still requires
numerical verification. Passing a synthetic tolerance is not a bound on trained
image quality. See [benchmarking](../docs/benchmarking.md).

## Quality evidence and its boundary

At a reported matched 45k point, Urban100 PSNR was **27.2615 for DUAL versus
27.215 for XG**. A separate 66k SSIM report favored DUAL. The owner subsequently
described DUAL as substantially better perceptually than QPA. These observations
motivated keeping DUAL as the quality reference; they are not proof of convergence,
equal seeds/recipes or isolated component causality.

**Reusable lesson:** optimize the unchanged useful computation before interpreting
an architecture's deployment cost. Then distinguish an execution improvement from
a representational change. Neither a slow first graph nor a successful custom
kernel proves that the remaining design space is exhausted.
