---
tags:
  - main
---
---
A vanilla RNN reads a sequence left → right.



**At each time step**:
* Reads $x^{\langle t\rangle}$.
* Updates hidden state $a^{\langle t\rangle}$.
* Predicts $y^{\langle t\rangle}$ (optional).

Used in tasks like:
* [[Sequence Model Notation (NER)]]
* [[Language Model & Sequence Generation]]

---

## Key Idea: Shared Parameters

The same parameters are reused at every time step. This allows the model to generalize learned patterns across word positions.

---

## Unidirectional Limitation

Prediction at time $t$ depends only on earlier inputs: $x^{\langle 1\rangle},...,x^{\langle t\rangle}$.
So it cannot use future context.
**Solution**: [[Bidirectional RNN (BRNN)]].

---

## Backpropagation Through Time

Training uses [[Backpropagation Through Time (BPTT)]].