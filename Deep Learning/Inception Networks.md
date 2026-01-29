---
tags:
  - extension
---
---
![[Inception-net.png]]
Inception networks apply multiple filter sizes in parallel instead of committing to just one.

---

## Why Inception?
Instead of choosing only one kernel size (e.g., $3\times3$ or $5\times5$), Inception combines them in parallel to capture features at different scales.

---

## Inception Module
**Parallel branches:**
1. $1\times1$ conv
2. $3\times3$ conv
3. $5\times5$ conv
4. Max pooling

Then **concatenate** all resulting feature maps.

---

## Role of $1\times1$ Convolution in Inception
[[Network-in-Network & 1x1 Convolution]] are used as a **bottleneck** before the expensive $3\times3$ and $5\times5$ convolutions to reduce depth.

✅ Reduces computation massively.
✅ Makes the module efficient.