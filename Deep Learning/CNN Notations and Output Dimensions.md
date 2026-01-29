---
tags:
  - main
---
---

For layer $l$ of a convolutional network:
- $f^{[l]}$ = filter size
- $s^{[l]}$ = stride
- $p^{[l]}$ = padding

---

## Input / Output Shapes

**Input volume:**
$$n_H^{[l-1]} \times n_W^{[l-1]} \times n_C^{[l-1]}$$

**Output volume:**
$$n_H^{[l]} \times n_W^{[l]} \times n_C^{[l]}$$

---

## Output Height & Width Formula

$$n_H^{[l]} = \left\lfloor \frac{n_H^{[l-1]} + 2p^{[l]} - f^{[l]}}{s^{[l]}} + 1 \right\rfloor$$

$$n_W^{[l]} = \left\lfloor \frac{n_W^{[l-1]} + 2p^{[l]} - f^{[l]}}{s^{[l]}} + 1 \right\rfloor$$

---

## Vectorized Implementation

For mini-batch training with $m$ examples:
$$A^{[l]} \in \mathbb{R}^{(m,\; n_H^{[l]},\; n_W^{[l]},\; n_C^{[l]})}$$

---

## Weights & Bias Shapes

**Weights:**
$$W^{[l]} \in \mathbb{R}^{(f^{[l]},\; f^{[l]},\; n_C^{[l-1]},\; n_C^{[l]})}$$

**Bias:**
$$b^{[l]} \in \mathbb{R}^{(1, 1, 1, n_C^{[l]})}$$

**Where:**
- $n_C^{[l]}$ = number of filters (channels) in layer $l$