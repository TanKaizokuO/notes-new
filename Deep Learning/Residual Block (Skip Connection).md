---
tags:
  - extension
---

---

## Problem: Vanishing / Exploding Gradients

![[traditional-block.jpg]]

In very deep networks:
- **Vanishing gradients:** early layers learn slowly.
- **Exploding gradients:** unstable training.

---

## Plain Network Block
**Forward pass:**
$$a^{[l]} = g(z^{[l]})$$
where:
$$z^{[l]} = W^{[l]}a^{[l-1]} + b^{[l]}$$


---

## Residual Block (Skip Connection)
**Residual idea:**
$$a^{[l]} = g(z^{[l]} + a^{[l-2]})$$

**General form:**
$$y = F(x) + x$$

- $F(x)$ = learned mapping
- $x$ = shortcut connection

---
