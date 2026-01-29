---
tags:
  - umbrella
---
---
# RNNs - Overview

Recurrent Neural Networks (RNNs) are models built for **sequence / temporal data**. They process inputs step-by-step and maintain a hidden state (memory).



**Common variants:**
* [[Vanilla RNN Model]]
* [[GRU (Gated Recurrent Unit)]]
* [[LSTM (Long Short-Term Memory)]]
* [[Bidirectional RNN (BRNN)]]
* [[Deep RNNs (Stacked RNN)]]

---

## Why Sequence Models?

Many applications naturally produce sequential data:
* Speech recognition
* Music generation
* Sentiment classification
* DNA sequence analysis
* Machine translation
* Video activity recognition
* Named Entity Recognition (NER) → see [[Sequence Model Notation (NER)]]

---

## Why not use a normal Neural Network?

If each word is one-hot encoded and fed into a standard NN:

### Problem 1: Variable length sequences
Each example may have different lengths ($T_x$, $T_y$). Padding all inputs to a fixed max length is inefficient.

### Problem 2: No parameter sharing
A naive NN doesn't reuse learned patterns across positions.

✅ **RNN solves both by**:
* Handling variable length sequences.
* Sharing parameters across time steps.

---

## Unidirectional Limitation

A standard RNN only uses left context.

**Example**:
* "Teddy Roosevelt was a great president."
* "Teddy bears are on sale!"

You need future words to disambiguate.
**Solution**: [[Bidirectional RNN (BRNN)]].