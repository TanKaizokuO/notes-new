---
tags:
  - main
---
---
![[IMG_0990.png]]
Pooling reduces spatial size (height/width), keeps the number of channels the same, and makes the representation more robust.

---

## Max Pooling

**Example:**
- Input: $4 \times 4$
- Filter $f=2$
- Stride $s=2$

**Output becomes:**
$$2 \times 2$$

---

## Hyperparameters of Pooling

Pooling layers have hyperparameters:
- filter size $f$
- stride $s$

> **Note:** Pooling has **no learnable parameters** (no weights, no gradient descent updates).

**Common choices:**
- $f=2, s=2$ (halves dimension)
- $f=3, s=2$

---

## Average Pooling
Average pooling can also be used, but **Max Pooling** is significantly more common in modern architectures.