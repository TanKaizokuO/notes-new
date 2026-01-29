---
tags:
  - main
---
---
Following are some advanced methods of [[Algorithm Optimization]]
## Mini-Batch Gradient Descent

Instead of using the entire dataset at once (Batch GD) or a single example (Stochastic GD), we use **mini-batches**.

### Dataset Representation
$$X = [x^{(1)}, x^{(2)}, x^{(3)}, ..., x^{(m)}]$$
$$Y = [y^{(1)}, y^{(2)}, y^{(3)}, ..., y^{(m)}]$$

### Forward Propagation (for a mini-batch)
For layer $l$:
$$Z^{[l]} = W^{[l]} X + b^{[l]}$$
$$A^{[l]} = g^{[l]}(Z^{[l]})$$

### Cost on Mini-batch
$$J^{\{t\}} = \frac{1}{m'} \sum_{i=1}^{m'} L(\hat{y}^{(i)}, y^{(i)}) + \frac{\lambda}{2m'} \sum \|W^{[l]}\|_F^2$$

**Where:**
- $m'$ = number of examples in one mini-batch

Similarly, perform backward propagation for the mini-batch.

### Choosing Mini-batch Size

1. **Batch Gradient Descent**
   - size $= m$
2. **Stochastic Gradient Descent (SGD)**
   - size $= 1$
   - Does **not always converge smoothly**.
3. **Mini-batch GD (practically used)**
   - Choose something in between.

**Rules of thumb:**
- Small dataset ($m \le 2000$): Use **Batch GD**.
- Typical mini-batch sizes: $2^6, 2^7, 2^8, \dots$ (64, 128, 256, ...).
- Ensure mini-batch fits in CPU/GPU memory.

---

## Gradient Descent with Momentum

Momentum speeds up learning by maintaining an [[Exponentially Weighted Moving Average]] of gradients.

On iteration $t$, compute $dW, db$ for the current mini-batch.

**Velocity Updates:**
$$V_{dW} = \beta V_{dW} + (1-\beta)dW$$
$$V_{db} = \beta V_{db} + (1-\beta)db$$

**Parameter Update:**
$$W = W - \alpha V_{dW}$$
$$b = b - \alpha V_{db}$$

> *Note: In some literature, $(1-\beta)$ is omitted.*

**Typical Hyperparameters:**
- $\beta = 0.9$
- Tune $\alpha$

---

## RMSProp

RMSProp (Root Mean Square Propagation) adapts the learning rate using an exponentially weighted average of squared gradients.

**Updates:**
$$S_{dW} = \beta S_{dW} + (1-\beta)dW^2$$
$$S_{db} = \beta S_{db} + (1-\beta)db^2$$

**Parameter Update:**
$$W = W - \alpha \frac{dW}{\sqrt{S_{dW}}}$$
$$b = b - \alpha \frac{db}{\sqrt{S_{db}}}$$

*(Division is element-wise)*

---

# 2. Adam Optimization Algorithm

Adam (Adaptive Moment Estimation) = **Momentum + RMSProp** with bias correction.

## Initialization
$$V_{dW}=0,\quad V_{db}=0$$
$$S_{dW}=0,\quad S_{db}=0$$

## On Iteration $t$
Compute gradients on current mini-batch: $dW, db$

### Momentum Part (1st Moment)
$$V_{dW} = \beta_1 V_{dW} + (1-\beta_1)dW$$
$$V_{db} = \beta_1 V_{db} + (1-\beta_1)db$$

### RMSProp Part (2nd Moment)
$$S_{dW} = \beta_2 S_{dW} + (1-\beta_2)dW^2$$
$$S_{db} = \beta_2 S_{db} + (1-\beta_2)db^2$$

## Bias Correction
$$V'_{dW}=\frac{V_{dW}}{1-\beta_1^t} ,\quad V'_{db}=\frac{V_{db}}{1-\beta_1^t}$$

$$S'_{dW}=\frac{S_{dW}}{1-\beta_2^t} ,\quad S'_{db}=\frac{S_{db}}{1-\beta_2^t}$$

## Parameter Update
$$W = W - \alpha \frac{V'_{dW}}{\sqrt{S'_{dW}} + \epsilon}$$
$$b = b - \alpha \frac{V'_{db}}{\sqrt{S'_{db}} + \epsilon}$$

## Hyperparameters
**Common Defaults:**
- $\beta_1 = 0.9$
- $\beta_2 = 0.999$
- $\epsilon = 10^{-8}$

**Tune:**
- Learning rate $\alpha$

---

# 3. Learning Rate Decay

Decaying learning rate improves convergence by taking smaller steps as the optimization approaches the minimum.

$$\alpha = \frac{\alpha_0}{1 + \text{decay\_rate} \times \text{epoch\_number}}$$

**Where:**
- $\alpha_0$ = initial learning rate

---

# 4. Local Optima and Plateaus

## Problem of Local Optima
In deep learning optimization, the loss surface may contain:
- Local optima
- Saddle points
- Flat regions

## Problem of Plateaus
A plateau is a region where gradients are very small, causing extremely slow learning.

**Characteristics:**
- Updates become tiny.
- Training stagnates for many iterations.

**Common Mitigation:**
- Momentum / Adam
- Better weight initialization
- Learning rate scheduling