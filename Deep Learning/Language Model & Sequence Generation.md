---
tags:
  - main
---
---

A language model estimates:
$$P(y^{\langle 1\rangle},y^{\langle 2\rangle},...,y^{\langle T\rangle})$$

It assigns higher probability to more realistic sentences.

---

## Example
![[Pasted image 20260118234306.png]]

| Sentence | Probability |
| :--- | :--- |
| "The apple and pair salad." | $3.2\times10^{-13}$ |
| "The apple and pear salad." | $5.7\times10^{-10}$ |

So the second is more likely.

---

## How to Build a Language Model with RNN


**Example**: "Cats average 15 hours of sleep a day <EOS>"

**Steps**:

1.  Tokenize words.
2.  Map each word → index / one-hot.
3.  Add `<EOS>` and `<UNK>`.
4.  Setup: $x^{\langle t\rangle}=y^{\langle t-1\rangle}$.
5.  Output distribution via softmax.
6.  Total loss = sum of losses across timesteps.

---

## Sentence Probability Factorization

$$P(y) = \prod_{t=1}^{T} P(y^{\langle t\rangle}\mid y^{\langle 1\rangle},...,y^{\langle t-1\rangle})$$

---

## After Training

You can generate text via:
* [[Sampling Novel Sequences]]
