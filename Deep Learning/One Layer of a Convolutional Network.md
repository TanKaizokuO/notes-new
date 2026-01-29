---
tags:
  - extension
---
---

A convolution layer applies multiple filters to an input volume to produce an output volume (feature maps) using [[Convolution, Padding and Stride]]

---

## Example: Single Conv Layer (2 filters)

**Input Volume:**
- $6 \times 6 \times 3$

**Filters:**
- $(3 \times 3 \times 3)_1$
- $(3 \times 3 \times 3)_2$

**Output:**
With stride = 1, padding = 0:
- Each filter produces a $4 \times 4$ output.
- Combined output (stacked):
$$4 \times 4 \times 2$$

This becomes $A^{[l]}$ for the next layer.

---

## Parameters in Conv Layer

For 2 filters each of size $3 \times 3 \times 3$:

**Weights:**
$$3 \times 3 \times 3 \times 2 = 54$$

**Bias:**
- 1 bias per filter $\rightarrow$ 2 biases

---

## Types of CNN Layers
- Convolution (Conv)
- Pooling (Pool)
- Fully Connected (FC)