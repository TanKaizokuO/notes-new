---
tags:
  - main
---
---

As in [[Neural Network]]
![[IMG_0963.jpg]]

## Model Overview

A **shallow neural network** has:

- Input layer: $a^{[0]} = X$
- One hidden layer
- Output layer: $\hat{y} = a^{[2]}$

It is called a **2-layer NN** because it has **two parameterized layers**.

---

## Parameters

$$W^{[1]}, b^{[1]}, W^{[2]}, b^{[2]}$$

```python
# W1: (n_h, n_x), b1: (n_h, 1)
# W2: (n_y, n_h), b2: (n_y, 1)
````

---

## Vectorized Implementation

The following equations represent the forward propagation steps for a neural network, comparing a single example (left) to a vectorized version for $m$ training examples (right).

### Single Training Example ($x_i$)

$$z^{[1]} = W^{[1]} x_i + b^{[1]}$$

$$a^{[1]} = \sigma(z^{[1]})$$

$$z^{[2]} = W^{[2]} a^{[1]} + b^{[2]}$$

$$a^{[2]} = \sigma(z^{[2]})$$

### When $m$ training examples ($X$)

$$Z^{[1]} = W^{[1]} X + b^{[1]}$$

$$A^{[1]} = \sigma(Z^{[1]})$$

$$Z^{[2]} = W^{[2]} A^{[1]} + b^{[2]}$$

$$A^{[2]} = \sigma(Z^{[2]})$$

---

## Random Initialization (Break Symmetry)

If weights are zero → all neurons learn the same thing 
Python
```
W1 = np.random.randn(n_h, n_x) * 0.01
b1 = np.zeros((n_h, 1))

W2 = np.random.randn(n_y, n_h) * 0.01
b2 = np.zeros((n_y, 1))
```
---
## Forward Propagation
### Hidden Layer

$$ Z^{[1]} = W^{[1]}X + b^{[1]} $$

$$ A^{[1]} = \tanh(Z^{[1]}) $$

Python

```
Z1 = W1 @ X + b1          # linear step
A1 = np.tanh(Z1)         # non-linearity
```

> `tanh` is preferred over sigmoid in hidden layers due to zero-centering.

### Output Layer (Binary Classification)

$$ Z^{[2]} = W^{[2]}A^{[1]} + b^{[2]} $$

$$ A^{[2]} = \sigma(Z^{[2]}) $$

Python

```
Z2 = W2 @ A1 + b2
A2 = 1 / (1 + np.exp(-Z2))  # sigmoid
```

> `A2` represents $\hat{y}$

---

## Cost Function (Binary Cross-Entropy)

$$ J = -\frac{1}{m} \sum \left[ y \log(\hat{y}) + (1-y)\log(1-\hat{y}) \right] $$

Python

```
m = Y.shape[1]
cost = -(1/m) * np.sum(
    Y * np.log(A2 + 1e-8) +
    (1 - Y) * np.log(1 - A2 + 1e-8)
)
```

---

## Why Non-Linear Activation?

Without activation:

$$ W^{[2]}(W^{[1]}X) = W'X $$

➡ Network collapses into **linear regression**.

Non-linearity enables **complex decision boundaries**.

---

## Backpropagation

### Output Layer Gradient

$$ dZ^{[2]} = A^{[2]} - Y $$

Python

```
dZ2 = A2 - Y
```

### Output Layer Parameters

$$ dW^{[2]} = \frac{1}{m} dZ^{[2]} A^{[1]T} $$

$$ db^{[2]} = \frac{1}{m} \sum dZ^{[2]} $$

Python

```
dW2 = (1/m) * dZ2 @ A1.T
db2 = (1/m) * np.sum(dZ2, axis=1, keepdims=True)
```

### Hidden Layer Gradient

$$ dZ^{[1]} = W^{[2]T} dZ^{[2]} \odot (1 - A^{[1]2}) $$

### Hidden Layer Parameters

$$ dW^{[1]} = \frac{1}{m} dZ^{[1]} X^T $$

$$ db^{[1]} = \frac{1}{m} \sum dZ^{[1]} $$
---

## Gradient Descent Update

$$ W := W - \alpha dW $$
---

## Prediction


```
Y_pred = (A2 > 0.5).astype(int)
```

---

## Activation Summary

| **Function** | **Formula**                     | **Use Case**     |
| ------------ | ------------------------------- | ---------------- |
| Sigmoid      | $\frac{1}{1+e^{-z}}$            | Binary output    |
| Tanh         | $\frac{e^z-e^{-z}}{e^z+e^{-z}}$ | Hidden layers    |
| ReLU         | $\max(0,z)$                     | Deep networks    |
| Leaky ReLU   | $\max(0.01z,z)$                 | Avoid dying ReLU |
