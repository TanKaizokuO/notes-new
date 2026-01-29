---
tags:
  - extension
---
---

# Distributed Representation (Embeddings)

Distributed representations learn **dense, low-dimensional vectors** where semantic meaning is captured by the geometry of the vector space.


---

## The Embedding Matrix
When training, the model learns an **Embedding Matrix** $E \in \mathbb{R}^{d \times |V|}$.
* $d$: Dimension (e.g., 300).
* $|V|$: Vocabulary size.
* **Lookup**: We do not multiply vectors; we simply look up the row corresponding to the word index.

---

## Historical Context: Neural Language Models
Before efficient algorithms like Word2Vec, embeddings were trained using **Neural Language Models**.
1.  **Input**: Context window of previous $k$ words.
2.  **Process**: Concatenate vectors $\to$ Hidden Layer $\to$ Softmax.
3.  **Output**: Predict probability of the next word.
* *Limitation*: Computationally expensive due to the massive output Softmax.

---

## Core Advantages
1.  **Low Dimensionality**: 100-300 dimensions vs millions in One-Hot.
2.  **Generalization**: Learning "orange juice" helps understand "apple juice".
3.  **Symmetry**: Handles OOV via subword information (in models like fastText).

---
