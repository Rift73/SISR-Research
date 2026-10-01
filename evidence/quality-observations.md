# Quality observations — reported, not audited experiments

[Home](../README.md) · [Evidence index](README.md)

These are the project owner's supplied tooltips and visual/training reports.
They were accepted without inspecting training logs, datasets or checkpoints.
Recipe/seed identity is unverified; no empirical seed-noise floor or convergence
claim is available. No model-generated image inspection is implied.

## Matched-step values supplied by the owner

All rows are **Reported**. Values are descriptive; differences are not significance
tests. Tooltip values are rounded. Separate rows can refer to different observations.

| Dataset / metric | Step | Reported comparison | Boundary on interpretation |
| --- | ---: | --- | --- |
| Urban100 PSNR | 22k | FOCUS 26.87; XG 26.77 | Early result; not a convergence verdict |
| Urban100 PSNR | 45k | DUAL 27.2615; XG 27.215 | Parameter count differs; not an isolated attention effect |
| Urban100 SSIM | 66k | DUAL 0.8237; XG 0.8222 | Separate observation from the PSNR row |
| Manga109 SSIM | 53k | DUAL 0.9228; XG 0.9224; QPA 0.9223; GRAFT 0.9222 | Small aggregate gaps do not localize or bound detail errors |
| Manga109 SSIM | 50k | DUAL2 0.9226; DUAL 0.9224 | Early rounded tooltip; not the latest comparative endpoint |
| Urban100 SSIM | 51k | DUAL2 0.8207; DUAL 0.8208 | Later owner report supersedes a continuing “DUAL2 worse” narrative |

**Latest update, 2026-10-01:** the owner reports DUAL2 now ahead of DUAL on both
Urban100 and Manga109 as training progresses. No new values, step or PSNR/SSIM
breakdown accompanied that update. Do not manufacture them or extrapolate convergence.

## Perceptual observations are a separate axis

| Comparison | Owner's observation | What remains unproven |
| --- | --- | --- |
| GRAFT-X versus GRAFT | Better perceived accuracy/texture despite little metric movement | Which component caused it; generalization across matched runs |
| XG versus GRAFT | Better aggregate metrics could coexist with less preferred small-detail fidelity | Metrics as a complete fidelity ranking; the three-way X/XG/GRAFT comparison was not wholly step-matched |
| QPA versus XG/DUAL | Missing or incorrect fine details, despite attractive metrics and latency; DUAL especially preferred | Shared matching, narrower widths or irregular-input padding as the isolated cause |
| DUAL2 | Current preferred architecture; increasingly encouraging metrics | Causality of reparameterization versus optimizer/EMA/run differences |

The XG/GRAFT visual report included matched 150k points; X in the three-way
comparison was at 97k. Calling the entire report “mismatched” would also be wrong.

No visual crops are included in this edition. Therefore readers cannot independently
verify the perceptual ordering from this repository. The reports still matter:
they define the owner's decision, not a universal ranking.

Relative wall-time labels in a training UI are not controlled throughput measures.
Unequal latest endpoints are not matched-step comparisons. A favorable early
trajectory can be useful evidence without being a final quality claim.
