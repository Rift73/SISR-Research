# Collaboration without treating agreement as evidence

[Home](../README.md) · [Attribution](../ATTRIBUTION.md)

The project owner chooses the objective and judges trained results. A design lead
proposes and selects architectures. An implementation/review role checks the actual
source, realizes the contract and reports measurements. Advice can challenge the
lead's reasoning, but the lead retains design judgment within the owner's requirements.

This division is useful only if facts remain independently checkable. An agreed
design is not a proof; a critic's preference is not a measurement. AI assistance
can produce persuasive explanations that rely on the wrong source or omit a cost.

## Three corrections worth copying into your workflow

| Initial reasoning | What review found | Better practice |
| --- | --- | --- |
| Reduce duplicated “owner” queries in DUAL's large context | The actual unshifted C64 source already had 262,144 spatial queries at LR512, not the larger count assumed | Resolve class inheritance and exact geometry before proposing an optimization |
| A small QPA SSIM gap implies small or sparse visual errors | Aggregate SSIM cannot identify the error pattern; the MSE argument also omitted its cross term | Separate supplied visual observations from deductions a metric cannot support |
| A cheap global retrieval core looks attractive | Projection, grouping, packing, rejection and backward were omitted or undercounted | Price the complete candidate and name what each diagnostic actually estimates |

Corrections should change the decision record, not disappear into a private chat.
Keep the original hypothesis, the disconfirming evidence and the narrowed conclusion.
Do not preserve repetitive polling, operational recovery or long debates that did
not change a mechanism, measurement or decision.

## A productive research handoff

Give the next person a [research card](../templates/research-card.md), the exact
behavioral contract, a fair comparator and the smallest decision-changing test.
Include the strongest counterargument. State which observations are supplied,
which facts are source-verified and which experiments have not happened.

More research effort should improve mechanism reasoning, antecedent coverage or
discriminating tests. It should not merely add speculative modules or multiply
review rounds until everyone agrees. A defensible “still unresolved” can be more
useful than a confident architecture name.
