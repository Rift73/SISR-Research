# SISR Research

**How we research better super-resolution architectures: hypotheses, measurements,
wrong turns, and the decisions they changed.**

This is a curated research notebook, not a model zoo, benchmark leaderboard or
dump of AI conversations. It follows the work from **WSR** through **GRAFT,
DUAL, QPA and DUAL2**, including the alternatives we stopped pursuing.
The purpose is to help you design better experiments for your own architectures.

The practical objective is faithful fine detail, especially manga, together with
PSNR/SSIM and fast inference. A model can improve the metric/latency tradeoff and
still be the wrong choice for that objective. That happened here.

## Start here

| What you want | Read |
| --- | --- |
| The research process you can reuse | [A decision-first research method](docs/research-method.md) |
| How the designs developed | [Journey and naming map](docs/journey.md) |
| The most instructive wrong turns | [Failure is not one category](cases/03-failure-atlas.md) |
| Why a fast, high-scoring model was not preferred | [The QPA tradeoff](cases/06-qpa.md) |
| How reparameterization returned in DUAL2 | [WSR returns](cases/07-dual2.md) and [folding math](docs/reparameterization.md) |
| Numbers you can inspect | [Evidence index](evidence/README.md) and [timing samples](evidence/latency.json) |
| What remains unresolved | [Current frontier](cases/08-current-frontier.md) |

Each case answers the same questions: **What was missing? What changed? What did
we observe? What did that establish—and not establish? What would change our mind?**

## Current position — 2026-10-01

- **DUAL2 is the project owner's preferred quality reference.** The owner reports
  that it now beats DUAL on Urban100 and Manga109. This is not a converged,
  independently audited or component-isolated result.
- **QPA remains an important efficiency result, not the preferred quality model.**
  Its measured prototype latency improved substantially; missing or incorrect
  fine detail was subsequently reported despite strong aggregate metrics.
- **No post-DUAL2 breakthrough has been demonstrated.** Coverage allocation and
  alternative detail transport are research hypotheses, not released successors.
- A fresh DUAL2 window comparison measured **56.43 ms for `[32,64]` versus
  86.88 ms for `[48,96]`**, under the specific seeded TensorRT contract in the
  [evidence index](evidence/README.md). This says nothing by itself about quality.

## How to read the claims

**Measured** means a recorded test under stated conditions. **Reported** means
the project owner's training/visual observation. **Derived** means algebra or
counting. **Hypothesis** means an explanation or prediction still needing a test.
These are different kinds of evidence, not interchangeable confidence badges.

Most deployment timings use **seeded, untrained weights**. They do not establish
trained image quality. Do not combine numbers from different sessions, shapes,
precisions or optimization levels into a leaderboard.

## Scope, provenance and credit

This first edition reconstructs existing research; its Git commits are publication
history, **not simulated historical development commits**. Selected timing samples
and sanitized source provenance are included. Full training code, weights, datasets,
private transcripts and engine binaries are not. See [provenance and limits](evidence/provenance.md).

Research was directed by Rift73, with AI assistance for research, implementation
and review. This does not imply every conclusion had independent agreement. See
[attribution](ATTRIBUTION.md) and [collaboration and corrections](docs/collaboration.md).

The components build on substantial prior work; start with the
[annotated primary references](REFERENCES.md). Original documentation is
**[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)**; third-party work
retains its own license. [Contribution guide](CONTRIBUTING.md) · [License](LICENSE)
