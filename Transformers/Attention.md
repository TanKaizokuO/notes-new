---
tags:
  - extension
---
---

# Self-Attention Mechanism

Think of the Self-Attention mechanism as a "fuzzy" dictionary lookup or a search engine inside the neural network.
* **Standard Dictionary**: You look up a **Key** to get a specific **Value** (1:1 mapping).
* **Transformer**: You use a **Query** to find the most relevant **Keys**, but instead of getting one result, you get a **weighted mixture** of all the **Values**.

---

## 1. Deriving Q, K, and V
For every token in the input sequence, we create three distinct vectors by multiplying the input embedding ($x$) by learned weight matrices:

$$q = x \cdot W^Q$$
$$k = x \cdot W^K$$
$$v = x \cdot W^V$$

* **Query ($q$)**: Represents the current word asking, *"What information do I need?"*.
* **Key ($k$)**: Represents every word advertising, *"Here is what I contain."*.
* **Value ($v$)**: The actual content or meaning of the word.

---

## 2. The Core Equation
The formula for **Scaled Dot-Product Attention** is:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{Q K^T}{\sqrt{d_k}}\right) V$$

---

## 3. Step-by-Step Breakdown

### Step A: The Dot Product ($Q \cdot K^T$)
We take the dot product between the Query of the current word and the Keys of all other words ($q \cdot k$).
* **Intuition**: Measures **similarity**.
* High positive score = Vectors point in same direction (High Attention).
* Zero/Negative score = Unrelated or opposite.

### Step B: Scaling ($\frac{1}{\sqrt{d_k}}$)
Divide scores by the square root of the key dimension ($d_k$).
* **Why?** Prevents dot products from becoming too large. Large numbers into Softmax result in tiny gradients (vanishing gradient problem), which kills training. Scaling keeps gradients healthy.

### Step C: The Softmax
Apply Softmax to turn scores into probabilities (0 to 1, summing to 1).
* **Result**: This is the **attention weight**.
* *Example*: For the word "it", the weight for "animal" might be 0.8, while "street" is 0.05.

### Step D: Weighted Sum ($\dots \times V$)
Multiply each word's Value vector ($v$) by its attention weight and sum them up.
* **Result**: A new vector representing the current word, **enriched with context** from relevant parts of the sentence.

---

## Plain English Summary
1.  **Query**: "I am 'it'. Who describes me?"
2.  **Key**: "I am 'street' (low match). I am 'animal' (high match)."
3.  **Softmax**: "Okay, I'll take 5% of 'street' and 80% of 'animal'."
4.  **Value**: *Constructs a new vector mixing 5% of street's meaning and 80% of animal's meaning.*

**Next**: [[Multi-Head Attention]] (Running this process in parallel to capture different relationships like grammar vs vocabulary).

---
