---
tags:
  - extension
---
---

After training a language model, we can generate new text.

This builds directly on:
* [[Language Model & Sequence Generation]]

---

## Sampling Algorithm

1.  **At timestep 1**: sample $\hat{y}^{\langle 1\rangle}$ from softmax distribution.
2.  **Feed it back**: $x^{\langle 2\rangle}=\hat{y}^{\langle 1\rangle}$.
3.  **Sample** $\hat{y}^{\langle 2\rangle}$.
4.  **Continue** until `<EOS>`.

**Implementation**: use `np.random.choice()` on the softmax probability vector.

---

## Avoid Sampling `<UNK>`

If `<UNK>` sampled:
* Reject and re-sample.