---
tags:
  - extension
---
---

# Word Analogy Reasoning

A defining property of high-quality embeddings is capturing semantic relationships via linear algebra.



---

## The Analogy Property
$$e_{man} - e_{woman} \approx e_{king} - e_{queen}$$

To solve "Man is to Woman as King is to ___?", we calculate:
$$Target = e_{king} - e_{man} + e_{woman}$$
We then search for the word vector closest to this $Target$ using **Cosine Similarity**.

---

## Linked Notes
- [[Cosine Similarity]]
- [[Distributed Representation (Embeddings)]]