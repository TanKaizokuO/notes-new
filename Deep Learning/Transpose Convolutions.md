---
tags:
  - extension
---
---
## Why do we need Transpose Convolution?
In semantic segmentation, we need to convert a **small feature map** into a **larger feature map**.

Example:
* 2×2 → 4×4
* 16×16 → 128×128

This is required because segmentation output must match the input resolution. Transpose convolution is a **learnable upsampling method** (unlike interpolation).

---

## Normal Convolution vs Transpose Convolution

![[oooaoghhfaj91.png]]

### Normal Convolution (downsampling)
* Example: input 6×6×3, filter 3×3×3, filters 5 → Output 4×4×5.
* ✅ spatial size decreases.

### Transpose Convolution (upsampling)
* Example: input 2×2, filter 3×3 → Output 4×4.
* ✅ spatial size increases.

---

## Parameters
Transpose convolution depends on:
* Filter size `f`
* Padding `p`
* Stride `s`

---

## How Transpose Convolution works (mechanical view)
Instead of sliding a filter on input, each input value "spreads" into a region of the output.

Steps:
1.  Take one input pixel value.
2.  Multiply it with every filter weight (f×f).
3.  Paste into output region.
4.  If regions overlap → **add values**.
5.  Ignore padded boundary.

---

## Why is it effective?
There are many ways to upsample (e.g., nearest neighbor, bilinear interpolation).

Transpose convolution is better because:
* ✅ weights are learnable
* ✅ optimized through backprop
* ✅ produces sharper segmentation boundaries in U-Net