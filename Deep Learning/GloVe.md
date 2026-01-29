---
tags:
  - extension
---
---

# GloVe (Global Vectors)

**GloVe** (Stanford) combines global matrix factorization (like LSA) with local context methods (like Word2Vec).

---

## Core Idea
It constructs a global **Co-occurrence Matrix** $X$, where $X_{ij}$ is how often word $j$ appears with word $i$.

## Objective
Learn vectors such that their dot product equals the log of their probability of co-occurrence:
$$\theta_i^T e_j \approx \log(X_{ij})$$

## Weighting Function
A weighting function $f(X_{ij})$ ensures that:
1.  Rare pairs don't introduce noise.
2.  Extremely frequent pairs (like "the-and") don't dominate the loss.

---

## Linked Notes
- [[Distributed Representation (Embeddings)]]
- [[Word2Vec]]