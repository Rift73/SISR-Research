# Contract: window names are not execution costs

[Home](../README.md) · [Current frontier](../cases/08-current-frontier.md)

In the measured DUAL2 experiment, “64 versus 96” meant the two-layer context
schedule **`[32,64]` versus `[48,96]`**, not every attention layer using one size.
Local G16 attention stayed unchanged. Channel width also stayed 96; do not confuse
the two uses of “C96.”

## Count actual partitions

For a one-dimensional length `L` and window `w`, this anchored partition uses
`ceil(L/w)` windows when `L>w`, placing the final full window against the far edge.
If `w` does not divide `L`, the final windows overlap. Square-window attention
processes those repeated token positions even though final outputs have one
value per image location.

For the **LR512 square** case, direct counting gives:

| Context side | Windows | Processed query rows | Attention pairs per head |
| ---: | ---: | ---: | ---: |
| 32 | 256 | 262,144 | 268,435,456 |
| 64 | 64 | 262,144 | 1,073,741,824 |
| 48 | 121 | 278,784 | 642,318,336 |
| 96 | 36 | 331,776 | 3,057,647,616 |

The combined context pair count grows **2.7567×**. That is neither the whole-model
FLOP ratio nor a latency prediction: projections, fixed local layers, reconstruction,
padding/packing and kernel efficiency all remain in the graph.

**Measured:** the complete paired engines grew from 56.42530 to 86.87865 ms under
the [recorded contract](../evidence/README.md#paired-measurements-with-included-samples).
Their execution-context allocations grew from 854,640,640 to 981,452,800 bytes.
Those allocations are not total inference VRAM or training memory.

## Training and deployment must both be named

An LR96 crop can expose a different partition from LR64, while a large page has
many more windows. Loading the same tensors does not preserve attended sets.
An offset may have no effect when the crop is no larger than its window.

The later Light-96 recipe requests an LR96 patch, batch 8 and accumulation 4
(effective batch 32), with `[48,96]` context. Its configuration was statically
validated; that is not a training, compilation or quality result.

Before experimenting, specify partition origin, final-window anchoring, masks,
output ownership, positional coordinates and tiled-image boundaries. Include the
cost of duplicated rows. Do not treat “same parameters” as “same capacity/function.”
