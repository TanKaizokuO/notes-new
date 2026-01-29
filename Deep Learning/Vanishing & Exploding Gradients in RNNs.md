---
tags:
  - main
---
---

During [[Backpropagation Through Time (BPTT)]], gradients can:
* Shrink exponentially → **vanishing gradient**.
* Grow exponentially → **exploding gradient**.

---

## Exploding Gradients
**Symptoms**:
* Parameters become huge.
* NaNs appear.

✅ **Fix**: Gradient clipping.

---

## Vanishing Gradients
**Harder issue**: Early inputs stop influencing later outputs.

**Fix using gated RNNs**:
* [[GRU (Gated Recurrent Unit)]]
* [[LSTM (Long Short-Term Memory)]]