---
tags:
  - main
---
---

GRU is designed to capture long-range dependencies and reduce vanishing gradients.
![[Pasted image 20260118235327.png]]
Builds upon:
* [[Vanishing & Exploding Gradients in RNNs]]

---

## Memory Cell

GRU introduces:
* $c^{\langle t\rangle}$: memory cell.
* $\tilde{c}^{\langle t\rangle}$: candidate update.
* Update gate: $\Gamma_u$.

If $\Gamma_u\approx 0$:
$$c^{\langle t\rangle}\approx c^{\langle t-1\rangle}$$
So memory persists across time steps.

---

## Full GRU
Uses another gate:
* $\Gamma_r$ relevance gate — controls how much $c^{\langle t-1\rangle}$ matters for new candidate.

---

## Practical Notes
* Gates are vectors (element-wise control).
* Values in (0,1), often near 0/1.