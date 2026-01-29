---
tags:
  - extension
---
---

## What is Network-in-Network?
Instead of only linear convolution filters, NiN applies **small neural networks** inside conv layers.
This leads to:
- $1\times1$ convolution
- Richer feature transformations

---

## $1\times1$ Convolution (Important)

A $1\times1$ convolution mixes channels at each pixel location without looking at neighbors.

**Transformation:**
Input: $H \times W \times C$
Output (using $C'$ filters): $H \times W \times C'$

---

## Uses of $1\times1$ Conv
✅ **Channel reduction** (bottleneck layer)
✅ Increase/decrease depth without changing spatial size
✅ Add nonlinearity between conv layers
✅ Efficient computation