# 03 — Failure is not one category

[Journey](../docs/journey.md) · Evidence: S06–S07 in [provenance](../evidence/provenance.md)

**Status:** the named successor packages are retired. Component-level explanations
are more limited than those dispositions; empirical i-LN benefit claims are withdrawn.

The useful artifact from a stopped design is not “don't try that.” It is a bounded
statement about what changed, what happened and which explanation remains open.

| Package | Observation | Supported disposition | Unsupported shortcut |
| --- | --- | --- | --- |
| Original SCION | Persistent reported quality deficit | Do not promote the tested package | “All factored position encodings fail” |
| SCION residual-control variant | Near recovery after removing sigma rescaling **and** changing LayerScale initialization | Residual/optimization explanations became more plausible | Assigning the recovery to either change alone |
| OCTAVE | Owner reported package failure, without a quantitative failure shape | Retire the package; causal diagnosis remains open | Blaming any one of its many jointly changed mechanisms |
| GRAFT-XC | Reported gains judged too modest | Insufficient advance for the owner's objective | Recasting the decision as solely a latency veto |
| FOCUS-PK | Early metric improvement, expensive deployment | Unattractive demonstrated quality/resource tradeoff | Calling it a convergence failure or saying P/K individually do not work |

## SCION: a valuable intervention, not an isolated explanation

The modified variant jointly removed a residual sigma multiplier and changed
LayerScale initialization from 0.1 to 1.0, while LayerScale remained learnable.
The observed recovery is informative. It does not identify which change caused
it, nor prove the retained positional representation has zero effect.

An optimized original-SCION graph later measured near parity with its GRAFT
reference. Thus an earlier slow native graph is not an intrinsic architectural
lower bound. The changed SCION training variant was a different, untimed object.

## OCTAVE: a redesign is not an isolated test

OCTAVE illustrates the opposite attribution problem from a small intervention:
it jointly changed normalization, biases, the stem's DC behavior, activation,
context support, reconstruction skip and cross-scale processing. A reported
failure of that bundle supplies no isolated diagnosis. GRAFT-X was designed
before that failure was known; it should not be narrated as a response to it.

## FOCUS: almost no parameters can still cost a great deal

FOCUS-PK combined cross-depth score history with selective attention support.
It added only 42 parameters, but retained optimized deployment measured
**86.1787 ms versus XG's 50.8093 ms** in a matched session: about 69.6% slower.
The owner's matched 22k screenshot reported Urban100 PSNR **26.87 versus 26.77**.
These are separate kinds of evidence: synthetic deployment cost and reported
early trained quality, not an end-to-end trained Pareto measurement.

History state, score precision, pruning/support handling and layout work were
real costs. A parameter count hid them. But neither this nor OCTAVE proves that
MAC counting is useless; it proves that it is incomplete.

## What to carry into the next design

Keep the full package as the unit of the original decision. If attribution matters,
design an intervention that distinguishes competing explanations. If two mechanisms
are intended to interact, a null result for either alone need not disprove the pair.

**Reusable lesson:** distinguish execution failure, quality regression, insufficient
improvement and unacceptable cost. Preserve negative evidence without expanding
its conclusion. These packages are retired here; their individual ideas are not
universally disproven. SST was not retired by this decision.
