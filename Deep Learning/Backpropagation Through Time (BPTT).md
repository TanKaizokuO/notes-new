---
tags:
  - main
---
---

BPTT is the training algorithm for RNNs.
![[Pasted image 20260118234152.png]]

---

## Key Concept

* **Forward pass**: left → right.
* **Backward pass**: right → left across time steps.

Gradients propagate across many time steps, which causes:
* [[Vanishing & Exploding Gradients in RNNs]]

BPTT is used to train:
* [[Vanilla RNN Model]]
* [[GRU (Gated Recurrent Unit)]]
* [[LSTM (Long Short-Term Memory)]]
*