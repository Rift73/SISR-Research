# Contract: when is a convolution fold actually lossless?

[Home](../README.md) · [DUAL2 case](../cases/07-dual2.md)

“Reparameterization” can mean either a different trainable representation of the
same deployed operator or an architecture change that can itself be fused. Ask
**equivalent to which function, at which boundary, in which arithmetic?**

## The current WSR-style 3×3 fold

Consider a linear chain with channel weights `W1`, spatial kernel `W2`, channel
weights `W3`, biases `b1,b2,b3`, and a parallel 1×1 skip `(S,bs)`. There is no
nonlinearity or input-dependent normalization between these operators.

For every spatial offset `(u,v)`, the folded kernel and bias are:

$$K_{u,v}=W_3W_{2,u,v}W_1+\mathbf{1}_{u=v=0}S,$$

$$b=W_3\left(\sum_{u,v}W_{2,u,v}b_1+b_2\right)+b_3+b_s.$$

Matrix dimensions and groups must match; these equations describe the dense,
stride-one case. The result is one affine 3×3 convolution, not a larger receptive
field. Current DUAL2 composes the kernel in FP32 for training and uses FP64 when
folding an export copy. These arithmetic choices are not bitwise-equivalence claims.

## The border trap

The expanded reference must **pad the input before the first biased 1×1**, then
apply the spatial 3×3 without additional padding. The first bias then exists at
the padded positions too, which gives the same bias sum at every output position.

If the first 1×1 runs before padding, its output's padded positions are zero rather
than `b1`. The border bias becomes position-dependent. A standard folded kernel
with one spatially constant bias will generally not reproduce it.

Likewise, two ordinary same-padded spatial convolutions do not automatically fold
to one standard convolution at all boundaries: the intermediate boundary truncation
can differ. Matching the interior is insufficient.

## Differentiable composition is not detachment

During training, compute `K(theta), b(theta)` without detaching the factors and
apply one convolution. Autograd differentiates through the composition. Expanded
intermediate feature maps are avoided, but extra factor parameters, optimizer
state and composition work remain.

Function-preserving initialization can set the expanded projections to duplicate/
average identities and initialize the effective kernel to the original kernel.
It does **not** make subsequent factor-space optimization identical to plain-kernel
optimization. Finite precision and optimizer epsilon also qualify simple update
scaling arguments.

## EMA is another function, not a bookkeeping detail

Let `F` fold factors into a kernel. In general:

$$F(\operatorname{EMA}(\theta))\ne\operatorname{EMA}(F(\theta)).$$

For two equally weighted scalar factor states `(1,1)` and `(2,2)`, multiplying
the averaged factors gives `1.5×1.5=2.25`; averaging their products gives `2.5`.
This algebra explains a possible confound. It does not prove why DUAL2 improved.

## Reusable acceptance checklist

- Name the original function and the proposed deployment function separately.
- Account for every bias, residual, pad, crop, stride, group and dilation.
- Check interior **and** corners/edges, regular **and** irregular sizes.
- Test input and parameter gradients, with nonzero branches.
- Compare low-precision differences against the original implementation's own
  precision floor; report the metric and scale, not simply “close.”
- Verify exported graph shape and complete latency independently of fold algebra.
- Never call changed widths, attention distributions or receptive fields lossless
  merely because the new operator has a valid internal fold.
