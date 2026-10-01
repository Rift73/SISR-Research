# Journey and naming map

[Home](../README.md)

This is a decision sequence, not a claim that every project is a direct descendant.
In particular, DRFT and SST are comparison/antecedent work, not children of WSR.
Dates describe the source-record periods, not invented commit timestamps.

```text
Light CNN research:   SPAN baseline → WideContextSR → WESR → WSR family
                                                               │
                                       training parameterization reused
                                                               ↓
Transformer research:
GRAFT v0 → v1
            ├─ SCION / OCTAVE (parallel alternatives)
            └─ X → XG
                    ├─ XC / FOCUS-PK
                    └─ DUAL
                        ├─ QPA → QPA2
                        └─ DUAL2 (preferred; reuses WSR-style parameterization)
```

This is a research relationship map, not a literal class-inheritance tree.
GRAFT was a separate project, not a WSR-derived transformer. Similarity between
WESR and GRAFT-X's exemplar ideas is not proof of direct design derivation.

| Stage | Question that drove it | What changed the next decision | Case |
| --- | --- | --- | --- |
| WSR origin, Sep 24 records | Can deployment savings buy more useful capacity at SPAN-like latency? | Full-engine fusion and single-stream measurements mattered more than MAC counts | [01](../cases/01-wsr.md) |
| WESR and WSR tiers, Sep 25–26 | Is coarse conditioning enough, or do we need exemplar transport? | Attention cost depended on layout; reported benefit depended on model budget | [01](../cases/01-wsr.md) |
| GRAFT v0/v1, Sep 26 | How can local and wider context coexist with practical training/deployment? | Normalization, positional representation and crop/deployment geometry needed explicit contracts | [02](../cases/02-graft.md) |
| SCION and OCTAVE, Sep 27–30 | Can a coherent redesign improve the trunk or reconstruction? | A joint residual intervention helped SCION; OCTAVE remained a package failure with unidentified cause | [03](../cases/03-failure-atlas.md) |
| GRAFT-X, XG and XC, Sep 29 | Can matched internal examples improve reconstruction at multiple scales? | Perception and metrics diverged; richer transport could give only modest gains | [04](../cases/04-retrieval.md) |
| FOCUS-PK, Sep 29–30 | Can history and selective attention improve evidence selection? | An early metric advantage came with a large measured deployment cost | [03](../cases/03-failure-atlas.md) |
| DUAL and exact-execution work, Sep 30 | Can spatial attention and regional channel interactions complement each other? | Encouraging quality reports; layout-aware kernels narrowed, but did not erase, cost | [05](../cases/05-dual.md) |
| DUAL-QPA, Sep 30–Oct 1 | Can the expensive widest-context attention be made much cheaper? | Strong resource results did not retain the owner's preferred fine-detail quality | [06](../cases/06-qpa.md) |
| QPA2 and DUAL2, Oct 1 | Can WSR-style training parameterization help without enlarging deployed convolutions? | The owner preferred DUAL2; the causal mechanism remains unresolved | [07](../cases/07-dual2.md) |
| Beyond DUAL2 and windows, Oct 1 | Where is safe computation reduction or better detail transport still possible? | No demonstrated breakthrough; larger context carried a measured cost | [08](../cases/08-current-frontier.md) |

## Names that otherwise cause mistakes

- **WSR** now names a consolidated family. Its Default/Compact tiers use the
  former exemplar-based WESR backbone; smaller tiers use context-only components.
  A rename/consolidation is not a new experiment.
- **GRAFT v1** is the base for the X → XG → DUAL line. Do not substitute v0's
  normalization or positional-bias description when explaining current DUAL.
- **DUAL-R / DUAL** refer here to the regional-channel extension of XG.
  **DUAL2** adds normal single-stage WSR-style convolution reparameterization.
- **QPA2** adds that reparameterization to QPA, not to intact DUAL attention.
  A historical two-linear-3×3-to-5×5 variant was superseded; it is not current DUAL2.
- **C96** in a tier name denotes channel width. **C32/C64** in the layer schedule
  denotes context-window side lengths. Always state which meaning is intended.
- Historical GRAFT tier labels were renamed. A result called “M” in an early
  record does not automatically describe a later factory with that label.

For evidence types and current statuses, use the [evidence index](../evidence/README.md),
not a name-based inference from an older report.
