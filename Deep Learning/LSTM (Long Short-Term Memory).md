---
tags:
  - main
---
---

LSTM is a gated RNN designed for long-term memory.

Like GRU, it addresses:
* [[Vanishing & Exploding Gradients in RNNs]]

---

## Key Difference from GRU
LSTM has two states:
* Hidden state $a^{\langle t\rangle}$
* Cell state $c^{\langle t\rangle}$

---
![[Pasted image 20260118235558.png]]
## Gates
* Forget gate $\Gamma_f$
* Update gate $\Gamma_u$
* Output gate $\Gamma_o$

**Candidate**: $\tilde{c}^{\langle t\rangle}$.

![[Pasted image 20260118235632.png]]

---

## Why LSTM Works
Cell state can carry information forward with minimal change if gates allow.

---

## Peephole Connection (Variant)
Sometimes gates depend on:
* $c^{\langle t-1\rangle}$ in addition to $a^{\langle t-1\rangle},x^{\langle t\rangle}$.

---

## GRU vs LSTM
* **GRU**: Simpler, faster, fewer gates.
* **LSTM**: More powerful, historically common default.