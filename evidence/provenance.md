# Provenance and reproducibility limits

[Home](../README.md) · [Evidence index](README.md)

This edition is a curated account of the owner's development records, source
inspection, benchmark artifacts and supplied observations. It is not a raw archive
or an independently reproduced study. Public source IDs below are stable labels;
private file locations and operational identifiers are intentionally omitted.

| ID | Underlying evidence | Used for |
| --- | --- | --- |
| S01 | Sep 24 WideContextSR design plan and source-level reasoning | Context-gate synthesis, fusion-led objective, antecedents |
| S02 | Corrected WideContextSR execution evidence, including auxiliary-stream rebuild | Final D8/S3 choice and two-build latency ratios |
| S03 | WESR successor feasibility record | Exemplar attention, selected configuration and whole-graph cost lessons |
| S04 | WSR consolidation handoff and family guide | Tier roles and distinction between rename and experiment |
| S05 | GRAFT v1 guide and source contract | Layer schedule, normalization, positional factors, crop/window regime |
| S06 | Cross-architecture postmortem **with subsequent factual qualifications** | SCION/OCTAVE/XC dispositions; limits on causal attribution |
| S07 | FOCUS failure-case record | Early reported quality gain versus measured cost |
| S08 | X/XG implementation contract, result report and corrected postmortem | Shared descriptors, independent temperatures, value identity and visual reports |
| S09 | DUAL implementation/latency handoff and owner observations | Regional channel design, parameter confound, paired timing and quality reports |
| S10 | Exact DUAL execution pilot, including source-identity corrections | Measured execution gain and invalidated owner-query premise |
| S11 | QPA prototype contract and feasibility report | Architecture changes, own-function checks and paired timing samples |
| S12 | QPA perceptual review **with subsequent factual qualifications** | Fine-detail report, MSE cross term and unisolated padding/width/matching changes |
| S13 | Current normal-WSR DUAL2 and QPA2 source, configuration and completion records | Fold dimensions, initialization, training execution and superseded 5×5 detour |
| S14 | Latest beyond-DUAL2 gap synthesis **and factual handoff** | Conditional EC direction, alternatives, optimizer/EMA and diagnostic limitations |
| S15 | DUAL2 window comparison report and paired timing arrays | Same-state geometry comparison, counts, numerical scope and included samples |
| S16 | Light-96 configuration change and static validation record | Requested patch/batch/accumulation/window settings, not execution proof |

The maintainer retains a private claim-to-record map. Earlier reports remain
preserved there; the later qualifications govern this public synthesis. An old
“DUAL pending” or “DUAL2 latency unmeasured” statement must not overwrite newer
evidence. Conversely, later observations must not be rewritten into what was
known when an earlier decision was made.

## What is independently inspectable here?

- [Timing samples](latency.json): sanitized compute times for two selected paired
  experiments, with enough metadata to recompute statistics and inspect variation.
- Equations and counting arguments: stated in the relevant contracts so readers
  can challenge assumptions rather than trust a conclusion by authority.
- [Primary references](../REFERENCES.md): links to antecedents, not copies of papers.
- This repository's genuine publication/correction history from its first release.

## What is not reproducible from this edition alone?

Engine rebuilding, trained PSNR/SSIM, perceptual rankings and training dynamics
require code, configurations, weights, data and evaluation conventions not bundled
here. Selected raw timings do not independently prove those missing links.
No artifacts were newly benchmarked just to produce this documentation.

We exclude private transcripts, raw logs, checkpoints, datasets, engine binaries,
training screenshots and original private Git history. A future code release
would need its own dependency/license and reproducibility review. Until then,
the contribution is a research method and an inspectable decision record—not a
claim of fully reproducible model performance.
