---
tags:
  - main
---
---

![[NN.jpg]]
A **Deep Neural Network** is a [[Neural Network]] neural network with **multiple hidden layers**, enabling it to learn complex non-linear mappings between input and output.

---

## Network Overview

- **Input features**:
  $$x = [x_1, x_2, x_3, x_4]^T$$
- **Layers**: $1, 2, 3, 4$
- **Output prediction**:
  $$\hat{y}$$

---

## Forward Propagation

### For any layer $l$

$$Z^{[l]} = W^{[l]}A^{[l-1]} + b^{[l]}$$

$$A^{[l]} = g^{[l]}(Z^{[l]})$$

**Where:**
- $W^{[l]}$: weight matrix for layer $l$
- $b^{[l]}$: bias vector for layer $l$
- $g^{[l]}$: activation function for layer $l$
- $A^{[0]} = X$ (input)

---

## Vectorized Form

When vectorized for $m$ training examples:

$$Z^{[l]}_{(n^{[l]},m)} = W^{[l]}_{(n^{[l]},n^{[l-1]})} \cdot A^{[l-1]}_{(n^{[l-1]},m)} + b^{[l]}_{(n^{[l]},1)}$$

### Broadcasting Bias
$$b^{[l]}_{(n^{[l]},1)} \rightarrow b^{[l]}_{(n^{[l]},m)} \quad \text{(via broadcasting)}$$

---

## Dimensionality (Shapes)

Let $n^{[l]}$ = number of neurons in layer $l$.

Then:
- **$Z^{[l]}$**: $(n^{[l]}, 1)$
- **$A^{[l]}$**: $(n^{[l]}, 1)$
- **$W^{[l]}$**: $(n^{[l]}, n^{[l-1]})$
- **$b^{[l]}$**: $(n^{[l]}, 1)$

---

## Why Deep Neural Networks?

1. **Shallow models** are limited in representing highly complex decision boundaries.
2. Deep networks can learn **hierarchical features**:
    - *Low-level* → edges/textures
    - *Mid-level* → patterns/shapes
    - *High-level* → objects/semantics
3. Better modeling of non-linear real-world data.

---

## Circuit Theory Intuition

- A deep network can be seen as a **composition of functions**.
- Like logic circuits:
    - Simple gates combine to build complex circuits.
    - Simple neurons combine to build complex decision functions.

---

## Backpropagation (Worked Example)
**Given:**
- Target: $y = 0.5$
- Learning rate: $\eta = 1$

![[Back_prop 1.jpg]]
## Parameters vs. Hyperparameters

In Deep Learning, it is crucial to distinguish between the variables the model learns and the settings you choose to control the learning process.

---

### 1. Parameters
**Definition**: Variables that the model **learns** autonomously from the training data during the training process.

**Examples**:
- Weights: $W^{[1]}, W^{[2]}, \dots$
- Biases: $b^{[1]}, b^{[2]}, \dots$

> These values are updated via **Backpropagation** and **Gradient Descent** to minimize the cost function.

---

### 2. Hyperparameters
**Definition**: Configuration variables set **before** the training process begins. They control "how" the model learns. We tune this hyperparameters ourselves to improve model performance in a process called [[Hyperparameter Tuning]]

**Examples**:
- **Learning Rate**: Denoted as $\alpha$ or $\eta$.
- **Iterations**: The number of training epochs.
- **Architecture**:
    - Number of hidden layers ($L$).
    - Number of hidden units ($n^{[l]}$) in each layer.
- **Choice of Functions**:
    - Activation functions (ReLU, Tanh, Sigmoid, etc.).

> **Analogy**: If parameters are the "knobs" the model turns to tune itself, hyperparameters are the "settings" you configure on the machine before turning it on.

---
