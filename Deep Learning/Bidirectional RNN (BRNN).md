---
tags:
  - main
---
---

BRNN uses both past and future context.

![[Pasted image 20260118235723.png]]

Solves limitation in:
* [[Vanilla RNN Model]]

![[Pasted image 20260118235838.png]]

---

## Key Idea

At timestep $t$, prediction depends on:
* Forward hidden state (left → right)
* Backward hidden state (right → left)

Thus it can use information from earlier words and later words.

Useful for:
* [[Sequence Model Notation (NER)]] because labels depend on both sides of a word.

---

## Blocks used inside BRNN
* Vanilla RNN
* [[GRU (Gated Recurrent Unit)]]
* [[LSTM (Long Short-Term Memory)]] (common)

---

## Disadvantage
Must have the full sequence first.

**Example**: In speech recognition, you may need to wait for the user to finish speaking.