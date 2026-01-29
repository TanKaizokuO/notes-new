---
tags:
  - extension
---
---

Regularization reduces overfitting by penalizing model complexity.

## Logistic Regression with L2 Regularization

$$J(w,b)=\frac{1}{m}\sum_{i=1}^{m} L(\hat{y}^{(i)},y^{(i)}) + \frac{\lambda}{2m}\|w\|_2^2$$

### L2 Norm
$$\|w\|_2^2 = w^T w = \sum_{j=1}^{n_x} w_j^2$$

## L1 Regularization

$$\|w\|_1 = \sum_{j=1}^{n_x} |w_j|$$

## $\lambda$ (Lambda)

$$\lambda = \text{regularization parameter}$$

Controls regularization strength:
- large $\lambda$ → stronger penalty → smaller weights
- small $\lambda$ → weaker penalty → more flexible model

---

## Regularization for Neural Networks

$$J(W^{[1]},b^{[1]},...,W^{[L]},b^{[L]}) = \frac{1}{m}\sum_{i=1}^{m} L(\hat{y}^{(i)},y^{(i)}) + \frac{\lambda}{2m}\sum_{l=1}^{L} \|W^{[l]}\|_F^2$$

### Frobenius Norm

$$\|W^{[l]}\|_F^2 = \sum_{i=1}^{n^{[l-1]}} \sum_{j=1}^{n^{[l]}} (W_{ij}^{[l]})^2$$

## How does regularization prevent overfitting?
- Penalizes large weights
- Encourages simpler decision boundaries
- Reduces sensitivity to noise
- Improves generalization

---

# Dropout Regularization

Dropout randomly shuts off neurons during training.

**Example:**
- $L = 3$
- keep-prob = 0.8

**Pseudo-code:**
```python
d3 = np.random.rand(a3.shape[0], a3.shape[1]) < keep_prob
a3 = np.multiply(a3, d3)
a3 /= keep_prob  # Inverted dropout technique
````
## Why does dropout work?
- Prevents co-adaptation of neurons
- Acts like training multiple smaller networks (ensemble effect)
- Improves generalization
---

# Data Augmentation

Used especially for image tasks.

**Examples:**

- Flipping
- Cropping
- Rotation
- Random noise injection

---

# Early Stopping

Early stopping reduces overfitting by stopping training when dev error starts increasing.

![[1_-37i5H3lie_x5299utHX4A.png]]

## Early Stopping Curve

- Training error keeps decreasing.
- Dev error decreases first, then starts increasing.
- Stop at point where dev error is minimum.    
✅ Helps with generalization.

### Downside

- Couples optimization and regularization together
- Might stop too early before reaching best parameters for training

---
