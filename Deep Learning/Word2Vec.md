---
tags:
  - extension
---
---
# Word2Vec

**Word2Vec** (Mikolov et al., 2013) is a family of shallow neural networks used to learn word embeddings efficiently. It relies on the **Distributional Hypothesis**: words in similar contexts have similar meanings.

---

## Architectures

### 1. CBOW (Continuous Bag of Words)
* **Task**: Predict **Target** word from **Context** words.
* **Method**: Averages context vectors.
* **Use Case**: Faster training, better for frequent words.

### 2. Skip-gram
* **Task**: Predict **Context** words from **Target** word.
* 
* **Use Case**: Works better with small datasets and rare words.

---

## Optimization: Negative Sampling
Standard Softmax is too slow ($|V|$ calculations). **Negative Sampling** converts the problem into **Binary Classification**:
* **Positive Pair**: (orange, juice) $\to$ Label 1
* **Negative Pair**: (orange, book) $\to$ Label 0

We update weights for the positive word and only $k$ (5-20) random negative words, drastically speeding up training.

---

## Linked Notes
- [[Distributed Representation (Embeddings)]]
- [[Cosine Similarity]]