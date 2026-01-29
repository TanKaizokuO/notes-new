---
tags:
  - main
---

---

**Paper**: *Man is to Computer Programmer as Woman is to Homemaker? Debiasing Word Embeddings*

Word embeddings trained on human text often inherit human biases (gender, ethnicity, religion).
* *Stereotype Example*: $Man : Programmer :: Woman : Homemaker$.



## The Debiasing Algorithm
The goal is to reduce unwanted bias while preserving useful semantic definitions (e.g., preserving $Girl:Boy$).

### Step 1: Identify Bias Direction
We calculate the "bias axis" by averaging the differences of definitional pairs:
$$g = \frac{1}{N} \sum (e_{she} - e_{he}) + (e_{girl} - e_{boy}) + \dots$$

### Step 2: Neutralize
For words that should be gender-neutral (e.g., *doctor, babysitter, pilot*):
* Project their vectors onto the bias axis $g$.
* Remove this projection to make them orthogonal to the bias direction.

### Step 3: Equalize
For definitional pairs (e.g., *grandmother, grandfather*):
* Adjust their vectors so they are equidistant from the set of gender-neutral words.
* This ensures that "grandmother" and "grandfather" have the same similarity score to "babysitter".

---

