---
tags:
  - extension
---
---

## Definition
Cosine similarity measures the similarity between two non-zero vectors by calculating the cosine of the angle $\theta$ between them.

It determines whether two vectors are pointing in roughly the same direction, **ignoring their magnitude** (length).


---

## Formula
Given two vectors $A$ and $B$:

$$\text{Cosine Similarity} = \cos(\theta) = \frac{A \cdot B}{||A|| \, ||B||} = \frac{\sum_{i=1}^{n} A_i B_i}{\sqrt{\sum_{i=1}^{n} A_i^2} \sqrt{\sum_{i=1}^{n} B_i^2}}$$

* **$A \cdot B$**: Dot product (Intersection of features).
* **$||A||$**: Magnitude (Length) of vector A.

---

## Interpretation of Values
The value ranges from **-1 to 1**:

| Value  | Angle ($\theta$) | Interpretation                                  |
| :----- | :--------------- | :---------------------------------------------- |
| **1**  | $0^\circ$        | **Identical orientation** (Perfect similarity). |
| **0**  | $90^\circ$       | **Orthogonal** (No correlation/similarity).     |
| **-1** | $180^\circ$      | **Opposite direction** (Perfectly dissimilar).  |

> [!NOTE] In NLP
> In text analysis (Bag of Words / TF-IDF), values usually range from **0 to 1** because word counts cannot be negative.

---

## Why use it? (vs. Euclidean Distance)
* **Euclidean Distance**: Measures the straight-line distance. Sensitive to document length.
    * *Example*: A short article about "Sports" and a long book about "Sports" might be far apart because of length.
* **Cosine Similarity**: Measures the angle. Robust to variations in length.
    * *Example*: The short article and long book will have a similarity near 1 because they point in the same "Sports" direction.

---

## Applications
* **Text Analysis**: Comparing document similarity (e.g., plagiarism detection).
* **Recommender Systems**: Finding users with similar tastes (Collaborative Filtering).
* **Information Retrieval**: Ranking search results.

---

