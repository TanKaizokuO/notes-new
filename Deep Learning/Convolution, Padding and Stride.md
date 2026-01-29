---
tags:
  - main
---


---

## Convolution ()
![[IMG_0988.png]]
- $6 \times 6$ matrix = image
- $3 \times 3$ matrix = filter (kernel)
- Output example = $4 \times 4$

Used for feature detection (e.g., vertical edges, horizontal edges).

---

## Note: Cross-Correlation vs Convolution

In deep learning, what we usually call "convolution" is technically **cross-correlation**, since the filter is not flipped (mirrored) before operation.

---

## Valid Convolution

**Definition:** No padding ($p=0$).

If input = $n \times n$ and filter = $f \times f$:

**Output:**
$$(n-f+1) \times (n-f+1)$$

---

## Padding Solves
![[IMG_0989.png]]


1.  **Shrinking of image size**: Prevents image from getting too small too quickly (e.g., $6\times6 \rightarrow 4\times4$).
2.  **Information Loss**: Preserves edge/border information that would otherwise be under-utilized.

---

## Same Convolution

**Definition:** Keeps output size equal to input size.

**Condition:**
$$(n+2p)-f+1=n \Rightarrow p=\frac{f-1}{2}$$

> **Note:** Padding $p$ is usually an integer, so $f$ is often chosen to be odd (3, 5, 7).

---

## Strided Convolution

Stride $s$ controls the step size of filter movement.

**Output dimension formula:**
$$\left\lfloor\frac{n+2p-f}{s}+1\right\rfloor$$

---

## Convolutions Over Volumes (RGB)

![[WhatsApp Image 2026-01-14 at 20.51.24 1.jpeg]]

**RGB Image Input:**
$$n \times n \times 3$$

**Filter:**
$$f \times f \times 3$$

- The number of channels in the input and filter must **match**.
- Convolution occurs in 3D.
- Multiple filters $\rightarrow$ Multiple feature maps $\rightarrow$ Outputs are stacked.