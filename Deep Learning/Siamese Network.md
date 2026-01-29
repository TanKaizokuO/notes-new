---
tags:
  - extension
---
---

![[Pasted image 20260117201803.png]]

A Siamese network learns an embedding function $f(x)$ to encode an input image into a vector (e.g., 128-dimensional).
Instead of producing a softmax output, the network focuses on the embedding space.

## Goal
* If $x_i$ and $x_j$ are the **same person**:    $$|| f(x_i) - f(x_j) ||^2 \text{ is small}$$
* If $x_i$ and $x_j$ are **different persons**:$$|| f(x_i) - f(x_j) ||^2 \text{ is large}$$

This embedding can later be used for verification, recognition, or clustering.

**Training Method**: [[Triplet Loss]].