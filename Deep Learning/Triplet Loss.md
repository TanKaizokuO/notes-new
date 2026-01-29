---
tags:
  - extension
---
---

Triplet loss trains a network to generate embeddings that effectively separate identities.

---
## The Triplet
Each training sample consists of 3 images:
1.  **Anchor (A)**
2.  **Positive (P)**: Same identity as A
3.  **Negative (N)**: Different identity than A

## Distances
* $d(A,P) = || f(A) - f(P) ||^2$
* $d(A,N) = || f(A) - f(N) ||^2$

## Goal & Loss Function
We want the positive distance to be smaller than the negative distance by a margin $\alpha$:
$$d(A,P) + \alpha \le d(A,N)$$

* **$\alpha$ (margin)**: Prevents the trivial solution where all embeddings collapse to zero.

**Loss Formula**:
$$L(A,P,N) = \max(d(A,P) - d(A,N) + \alpha, 0)$$

**Overall Objective**:
$$J = \sum L(A^{(i)}, P^{(i)}, N^{(i)})$$

> [!TIP] Training Detail
> Random triplets are often too easy to satisfy. Effective training requires choosing **"hard triplets"** where the model currently fails.

**Related**: [[Face Verification as Binary Classification]]