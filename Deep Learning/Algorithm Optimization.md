---
tags:
  - main
---
---
# DL Optimisation

Deep Learning optimisation focuses on improving training performance, generalization, and stability by using correct data splits, diagnosing bias/variance, and using regularization techniques.

---

## Dataset Split (Big Data Era)

Data is split into:
- **Training set**
- **Dev / Validation set**
- **Test set**

> Important: Dev and test sets must come from the **same distribution**.

In modern deep learning, dataset split should be intuitive but sufficient to properly validate and test.

---

## Bias / Variance Diagnosis

### Case 1: Overfitting (High Variance)
- Train error = **1%**
- Dev error = **11%**
✅ Model fits training well, fails to generalize.

### Case 2: Underfitting (High Bias)
- Train error = **15%**
- Dev error = **16%**
✅ Model is too simple / not learning enough.

### Case 3: High Bias + High Variance
- Train error = **15%**
- Dev error = **30%**
✅ Model is bad on training and worse on dev.

### Case 4: Low Bias + Low Variance
- Train error = **0.5%**
- Dev error = **1%**
✅ Ideal scenario (assuming Bayes/optimal error ≈ 0%)

---

## Basic Recipe for Deep Learning

### If High Bias (high train error):
- Use a **bigger network**
- **Train longer**
- Try different **NN architecture**

### If High Variance (high dev error):
- Get **more data**
- Apply **regularization**
- Try different **NN architecture**

---

## In [[Deep Neural Networks]] : Bias-Variance Tradeoff

In traditional ML, reducing bias often increases variance and vice versa.

In Deep Learning, there is **less strict bias-variance tradeoff** compared to classical models, especially with large datasets and large networks.

---
###  [[Regularization]]?

Regularization is a technique used to **prevent overfitting** (high variance) by adding a penalty to the cost function. This discourages the model from becoming too complex or relying too heavily on any specific input feature.

---
# Normalizing the Training Set

Normalization improves convergence speed and stability.

## Step 1: Subtract Mean

$$\mu = \frac{1}{m}\sum_{i=1}^{m} x_i$$

$$X := X - \mu$$

## Step 2: Normalize Variance

$$\sigma^2 = \frac{1}{m}\sum_{i=1}^{m} x_i^2$$

$$X := \frac{X}{\sigma}$$

> Use the same $\mu$ and $\sigma$ from training set to normalize dev and test sets.

We also use normalization between layers (on Z[l]) called [[Batch Normalization]]  

---

# Vanishing / Exploding Gradients

Occurs in deep networks due to repeated multiplication through many layers.

- **Vanishing gradients**: gradients become extremely small → slow learning
- **Exploding gradients**: gradients become extremely large → instability

✅ **Solution**: Weight initialization (e.g. Xavier/He)

---

# Numerical Approximation of Gradient

$$f'(a)=\lim_{h\to 0} \frac{f(a+h)-f(a-h)}{2h}$$

---

# Gradient Checking

Used to verify backpropagation implementation.

## Steps

1. Take all parameters:

   $$W^{[1]}, b^{[1]}, ..., W^{[L]}, b^{[L]}$$	  & Reshape into a vector theta
$$\theta$$

2. Take gradients:
   
$$dW^{[1]}, db^{[1]}, ..., dW^{[L]}, db^{[L]}$$
 
   & Reshape into a vector d theta$$d\theta$$
2. For each $i$:
   $$d\theta_{approx}[i] = \frac{J(\theta_1,...,\theta_i+\epsilon,...,\theta_n) - J(\theta_1,...,\theta_i-\epsilon,...,\theta_n)}{2\epsilon}$$

## Gradient Check Metric

$$check = \frac{\|d\theta_{approx} - d\theta\|_2}{\|d\theta_{approx}\|_2 + \|d\theta\|_2}$$

**Example:**

- if $\epsilon = 10^{-7}$
- then check ≈ $10^{-7}$ is excellent

## Notes / Best Practices

- Do **NOT** use gradient checking during training; use it only for debugging.
- If gradient check fails, inspect component-wise to find bugs.
- Regularization gradients must be included correctly.
- Gradient checking does **not work well with dropout**.
- Run at random initialization and again after some training.