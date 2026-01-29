---
tags:
  - extension
---
---

Batch Normalization (Batch Norm) normalizes intermediate layer values to reduce internal covariate shift and speed up training.

## Mathematical Formula

Given activations in layer $l$ for a mini-batch: $Z^{[l](1)}, \dots, Z^{[l](m)}$

### 1. Compute Mean & Variance
$$\mu = \frac{1}{m}\sum_{i=1}^{m} Z^{[l](i)}$$
$$\sigma^2 = \frac{1}{m}\sum_{i=1}^{m}(Z^{[l](i)} - \mu)^2$$

### 2. Normalize
$$Z^{[l](i)}_{norm} = \frac{Z^{[l](i)} - \mu}{\sqrt{\sigma^2 + \epsilon}}$$

### 3. Scale and Shift
$$\tilde{Z}^{[l](i)} = \gamma Z^{[l](i)}_{norm} + \beta$$

**Note:**
- $\gamma, \beta$ are **learnable parameters**.
- If $\gamma = \sqrt{\sigma^2+\epsilon}$ and $\beta = \mu$, the network recovers the original identity mapping.

---

## Implementation Pipeline

1. **Linear Step**: $Z^{[l]} = W^{[l]}A^{[l-1]} + b^{[l]}$
2. **Batch Norm**: $\tilde{Z}^{[l]} = BN(Z^{[l]})$
3. **Activation**: $A^{[l]} = g^{[l]}(\tilde{Z}^{[l]})$

> **Note**: Bias $b^{[l]}$ is usually omitted/redundant because Batch Norm's $\beta$ parameter handles the shift.

---
**Covariate Shift** occurs when the distribution of the input data (1$x$) changes, but the relationship between inputs and outputs (2$y$) remains the same.3

### 1. General Example (The "Cat" Problem)

Imagine you train a neural network to detect cats using only images of **black cats**.

- Later, you test it on images of **colored cats**.
- The fundamental concept of a "cat" hasn't changed (the label $y$ is the same), but the distribution of pixel values ($x$) is completely different.
- The model will fail because it wasn't trained on this new distribution. This is **Covariate Shift**.

### 2. "Internal" Covariate Shift (In Deep Networks)

In a deep neural network, this happens continuously **between layers** during training.4

- **The Problem:** Layer 2 relies on the output of Layer 1. As Layer 1 updates its parameters ($W^{[1]}, b^{[1]}$), its output distribution changes.
- **The Effect:** Layer 2 is constantly trying to learn from a "moving target."5 It has to keep re-adapting to the changing statistics of Layer 1, which slows down training.

### 3. How Batch Norm Fixes It

Batch Normalization acts as a stabilizer.6 By forcing the output of every layer to have a mean of $\approx 0$ and variance of $\approx 1$:

- Later layers (e.g., Layer 3 or 4) see a **consistent distribution** of inputs, regardless of how much the weights in early layers wiggle around.
- This decouples the layers, allowing each to learn more independently and enabling higher learning rates.
---
## Why does it work?
- **Covariate Shift**: Stabilizes learning by handling shifting input distributions.
- **Speed**: Allows for higher learning rates.
- **Regularization**: Adds slight noise (due to mini-batch statistics), acting as mild regularization.

---

## Test Time Behavior
At test time, mini-batch mean/variance are unavailable.
Use  [[Exponentially Weighted Moving Average]] of $\mu$ and $\sigma^2$ collected during training:

$$\mu_{test} \approx EWMA(\mu_{train})$$
$$\sigma^2_{test} \approx EWMA(\sigma^2_{train})$$
