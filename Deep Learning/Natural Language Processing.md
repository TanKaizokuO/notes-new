---
tags:
  - umbrella
---
---

# 🗂️ NLP & Data Representation Index

Welcome to the knowledge base for Natural Language Processing data representation. This index organizes notes from raw data preprocessing to advanced deep learning embeddings.

---

## 🏗️ Foundations
Before processing text, we must understand how to handle data and the general pipeline.
* [[Categorical Encoding]] — Handling non-text categorical data (One-Hot vs. Ordinal).
* [[Text Representation]] — The high-level pipeline: converting text to numbers.

---

## 🧮 Vectorization Techniques
Techniques to convert text into numerical vectors.

### 1. Sparse / Counting Methods
Simple, interpretable, but high-dimensional approaches.
* [[Basic Vectorization Approaches]] — Covers One-Hot, Bag of Words (BoW), and TF-IDF.

### 2. Dense / Deep Learning Methods
Advanced approaches that capture semantic meaning.
* [[Distributed Representation (Embeddings)]] — The concept of dense vectors and the distributional hypothesis.

---

## 🤖 Embedding Models
Specific architectures used to learn distributed representations.
* [[Word2Vec]] — The family of models including **CBOW** and **Skip-gram**, utilizing **Negative Sampling**.
* [[GloVe]] — Global Vectors for Word Representation (Matrix Factorization + Context Window).

---

## 📐 Core Concepts & Math
The mathematical principles that make embeddings work.
* [[Cosine Similarity]] — How we measure distance/similarity between word vectors.
* [[Word Analogy Reasoning]] — How algebra ($King - Man + Woman \approx Queen$) works in vector space.

---

## 🚀 Workflow & Applications
Practical usage and ethical considerations.
* [[NLP and Word Embeddings Workflow]] — The process of Pre-training, Transfer Learning, and Visualization (t-SNE).
* [[Sentiment Classification with Embeddings]] — Using embeddings for tasks like review analysis.
* [[Debiasing Word Embeddings]] — Identifying and removing human bias (gender, race) from models.