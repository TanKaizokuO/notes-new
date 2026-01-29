---
tags:
  - extension
---
---

For multi-class classification ($C$ classes) [[Deep Neural Networks]], the output layer predicts probabilities for each class:
$$P(\text{class}_1|x), \dots, P(\text{class}_C|x)$$

---

## Softmax Layer

Given the final linear output $Z^{[L]}$:

1. **Exponentiate**:
$$t_i = e^{Z_i^{[L]}}$$

2. **Normalize**:
$$A_i^{[L]} = \frac{t_i}{\sum_{j=1}^{C} t_j} = \frac{e^{Z_i^{[L]}}}{\sum_{j=1}^{C}e^{Z_j^{[L]}}}$$

**Property**: The sum of all outputs is 1.
$$\sum_{i=1}^{C}A_i^{[L]} = 1$$

---

## Worked Example

$$Z^{[L]} = \begin{bmatrix} 5 \\ 2 \\ -1 \\ 3 \end{bmatrix} \implies t = \begin{bmatrix} e^5 \\ e^2 \\ e^{-1} \\ e^3 \end{bmatrix} \approx \begin{bmatrix} 148.4 \\ 7.4 \\ 0.4 \\ 20.1 \end{bmatrix}$$

Sum $\approx 176.3$

$$A^{[L]} = \frac{t}{176.3} \approx \begin{bmatrix} 0.842 \\ 0.042 \\ 0.002 \\ 0.114 \end{bmatrix}$$

---

## Loss Function (Cross-Entropy)

$$L(\hat{y}, y) = -\sum_{j=1}^{C} y_j \log(\hat{y}_j)$$

---

## Relation to Logistic Regression
-  Softmax is the generalization of Logistic Regression to $C > 2$ classes.
-  If $C=2$, Softmax reduces mathematically to the Sigmoid function.