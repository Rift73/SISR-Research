# A decision-first research method

[Home](../README.md) · [Cases](journey.md)

The unit of useful research is not a new block. It is a **decision whose outcome
can change when evidence arrives**. A paper search, derivation or benchmark that
cannot affect a decision is often premature.

## 1. State the objective before choosing a mechanism

Our objective is faithful reconstruction, with special attention to manga detail,
plus PSNR/SSIM and inference efficiency. Inference speed takes priority among
resource costs, **conditional on acceptable quality**. Training cost still matters.

The owner's illustrative preference: 15 to 16.5 GB for 57 to 52 ms was attractive;
15 to 22 GB for the same latency gain was not. These are preference anchors, not
a universal exchange rate, memory cap or measured model comparison. The original
example did not specify training versus inference memory: a real decision must.

Write down the error types that matter: missing strokes, wrong repeated texture,
unfaithful detail, border artifacts. Do not replace this objective with whichever
benchmark is easiest to improve. Conversely, do not dismiss a measured metric
improvement merely because it does not decide adoption.

## 2. Draw the information path

For every proposed operator, answer:

1. What evidence enters it that the baseline could not use as directly?
2. What distinguishes the queries, keys and transported values?
3. Where are detail, phase, intensity or positional distinctions compressed?
4. Can the network reject a bad match, and is that rejection truly zero after
   output projection and bias?
5. Which gradients can reach it at initialization and after learning begins?

For example, four output slots do not necessarily provide four independent
matching decisions. QPA preserves distinct value slots while sharing matching
structure and narrowing several projections. Calling that merely a reshape
hides the architectural tradeoff. See [QPA](../cases/06-qpa.md).

## 3. Make the strongest alternative concrete

Compare a complete candidate with both a serious different mechanism and the
**equally optimized unchanged baseline**. Do not force every successor to keep
the same backbone. Also do not replace a known-good design with a stack of
unpriced ideas and call the collection a breakthrough.

Distinguish four claims:

| Claim | What would support it? |
| --- | --- |
| New primitive | A genuinely different operation and a careful prior-art comparison |
| Architectural contribution | A coherent organization or interaction addressing a demonstrated limitation |
| Systems contribution | The same stated function executed better under a verified numerical contract |
| Diagnostic contribution | A test or explanation that changes what should be built next |

Combining known components can be valuable architectural work. It does not prove
primitive novelty. A new name is evidence of neither novelty nor usefulness.

## 4. Count the whole execution

Account for projection, packing, padding, normalization, score storage, readout,
layout conversion and backward—not just the attractive attention equation.
Count temporary tensors' lifetimes, not their sizes alone. An activation inventory
is not peak memory; a kernel sum is not guaranteed whole-engine latency.

Ask what the backend can actually fuse. Preserve bias, masks and required precision
when testing a fast path. If that contract is unsupported, say so; silently dropping
the bias changes the experiment. Our GRAFT work used Flash where supported and a
different path for local attention with dense positional bias.

Use [the benchmark contract](benchmarking.md) before interpreting a speed claim.

## 5. Choose the smallest test that separates explanations

| Question | Informative test | What it cannot establish |
| --- | --- | --- |
| Is the new math implemented correctly? | Independent forward/gradient reference; active nonzero branches; borders | Trained quality |
| Is deployment feasible? | Matched full-engine build, fusion inspection and paired timing | General hardware optimality |
| Why did a package fail? | A targeted intervention, including interaction controls when needed | All causes from one changed toggle |
| Does a trained model depend on a branch? | Disable that branch in the trained model | Quality of a model trained without it |
| Did the target improve? | Representative visual comparisons and metric trajectories | Isolated causality without matched controls |

A joint experiment can be the right choice when components are intended to work
together. Its result answers a package-level question. Do not invent component-level
attribution afterward. Equally, requiring every radical design to preserve every
baseline path can rule out the very change you want to investigate.

## 6. Predeclare the decision, not a convenient story

Specify what would cause you to proceed, redesign or stop. A deadline, memory limit
or relative slowdown allowance from one experiment does not silently become a
universal project rule. Do not invent a noise margin such as 0.05 dB without evidence.

The first WSR selection changed when an uncontrolled auxiliary-stream setting was
corrected. The corrected comparison, not the earlier favorite, governed the result.
QPA later passed an efficiency feasibility gate but did not become the preferred
quality architecture. Both are examples of the process doing its job.

## 7. Preserve conclusions at the right level

Record the observation, plausible explanations, confounds, disposition and cheapest
remaining discriminating test. Retire a package without declaring its components
universally useless. Keep implementation failures separate from model failures.

If evidence is missing, write **unknown**. Unknown is neither non-inferiority nor
proof that the design space is exhausted. Stop a bounded research round when it
has produced a defensible decision; do not generate endless tests to avoid uncertainty.

Use the [research-card template](../templates/research-card.md) for your own work.
