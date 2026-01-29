---
tags:
  - extension
---
---

# Basic Vectorization Approaches

These techniques convert text into vectors using **vocabulary statistics** (counting). They are simple but result in **sparse, high-dimensional** vectors.

---

## 1) One-Hot Encoding (OHE)
Represent each word as a binary vector of length $|V|$ (Vocabulary size).
* **Dog** $\to [1, 0, 0, 0, 0, 0]$
* **Issues**: Sparsity, no semantic similarity, Out of Vocabulary (OOV) errors.

---

## 2) Bag of Words (BoW)
Represent a document by the **frequency** of each word.
* "Dog bites man" $\to [1, 1, 1, 0, 0]$
* **Pros**: Fixed length representation.
* **Cons**: Loses word order, sparsity remains.

---

## 3) TF-IDF
**Term Frequency $\times$ Inverse Document Frequency**
Scores words based on importance.
* Words frequent in *one* document but rare *globally* get **high scores**.
* Common words (stopwords) get **low scores**.

$$TFIDF(t,d) = TF(t,d) \times \log\left(\frac{\text{total docs}}{\text{docs with } t}\right)$$

---
