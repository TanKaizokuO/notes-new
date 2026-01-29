---
tags:
  - main
---
---

# Sentiment Classification with Embeddings

Using embeddings allows classifiers to generalize better with less data.

## 1. Simple Average Model
* **Method**: Average the embeddings of all words in a review and feed to a classifier.
* **Pros**: Fast.
* **Cons**: Ignores **word order** (e.g., "Good taste" vs "Not good taste").

## 2. RNN/LSTM Model
* **Method**: Feed embeddings sequentially into an RNN.
* **Pros**: Captures order and dependencies (negation, sarcasm).
* **Cons**: Slower to train.

---

## Linked Notes
- [[NLP and Word Embeddings Workflow]]
- [[Distributed Representation (Embeddings)]]