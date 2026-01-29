---
tags:
  - main
---
---
Hyperparameter tuning is the process of selecting the best training and configuration settings that maximize dev-set performance.

---

## Important Hyperparameters (Priority)

### 1. Most Important
- **Learning rate** $\alpha$

### 2. Second Most Important
- $\beta$ for **Exponentially Weighted Average (Momentum)**
- Hidden units (Mini-batch size)

### 3. Others (Lower Priority)
- Number of layers
- Learning rate decay

### 4. Usually Never Tune
- $\beta_1 = 0.9$
- $\beta_2 = 0.999$
- $\epsilon = 10^{-8}$

---

## Search Strategy

### Random Search > Grid Search
- Try random values first rather than a fixed grid.
- **Coarse to Fine**:
    1. Sample broadly to find a promising region.
    2. Zoom into that region and sample more densely.

---

## Sampling Techniques

### 1. Learning Rate (Log Scale)
Sampling $\alpha$ linearly is inefficient. Use **log scale**:

Choose $r \in [a, b]$ where:
$$a = \log_{10}(\alpha_{lower})$$
$$b = \log_{10}(\alpha_{upper})$$

Then:
$$\alpha = 10^r$$

### 2. [[Exponentially Weighted Moving Average]] Parameter $\beta$
For $\beta \approx 1$ (e.g., $0.9 \to 0.999$), sampling linearly is poor.
Sample $1-\beta$ in log scale instead.

Choose $r \in [\log_{10}(1-\beta_U), \log_{10}(1-\beta_L)]$

Then:
$$1-\beta = 10^r \implies \beta = 1 - 10^r$$

---

## Training Strategy

1. **Babysitting (Panda)**: Manually monitor and adjust one model at a time.
2. **Parallel (Caviar)**: Train many models simultaneously with different configs (requires high compute).


---
