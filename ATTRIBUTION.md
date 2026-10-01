# Attribution and scope of reuse

## Human direction and AI assistance

**Rift73** directed the research, selected its practical priorities, conducted the
reported training, supplied benchmark screenshots and judged fine-detail quality.
Those trained-quality observations belong to the owner's reports; they are not
independent evaluations by this repository's authors or tools.

**Claude Opus 5.5** served as architectural research/design lead. **Fable 5.1** was
consulted on selected consequential questions. **Codex** implemented and measured
the project work, reviewed source/math/measurement claims, and assembled this
public documentation with an editorial review from the lead.

Not every design was seen by every advisor; this is not a claim of unanimous or
independent expert validation. The current publication round did not run new
model experiments. [Collaboration lessons](docs/collaboration.md) explain how
factual corrections affected the work.

## Prior work

The [reference list](REFERENCES.md) credits the primary antecedents used in this
edition. Wide activation, structural reparameterization, conditional modulation,
window/channel attention and internal exemplar retrieval predate this project.
Their combination here is not a first-invention claim.

The implementation work used the traiNNer-redux training framework, PyTorch,
TensorRT and GPU-kernel tooling. Their authors retain credit and their licenses
remain separate. DRFT and SST are names from the owner's preceding research
records, included as comparators—not claimed here as new public inventions.

## License and attribution

Copyright © 2026 Rift73. Original documentation and the original selection and
arrangement of project evidence in this repository are licensed under
**Creative Commons Attribution 4.0 International**. See [LICENSE](LICENSE) and
the [official deed](https://creativecommons.org/licenses/by/4.0/).

Suggested credit: **Rift73, “SISR Research,” with the repository URL and the
version or commit you used.** Indicate changes when adapting the documentation.

Linked papers, third-party code, names and any third-party material retain their
own rights and licenses. This repository does not relicense them. No model
implementation, weights or paper PDFs are distributed in this edition.
