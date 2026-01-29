---
tags:
  - main
---
---
## Why Dimensionality Reduction?
Dimensionality reduction aligns with **Occam’s Razor**: prefer simpler solutions when they explain the data well. It helps build:
- simpler models
- better generalization
- easier interpretability and deployment

> Idea: remove redundant / less informative features to simplify representation.

---

## Occam’s Razor Philosophy
- "Entities must not be multiplied beyond necessity"
- In ML: prefer simpler models unless added complexity improves accuracy significantly.
- Complex models → higher risk of **overfitting**.
- Simpler models → easier to interpret/debug/deploy.

---

## Curse of Dimensionality
As dimensions ↑:
- data becomes sparse
- space expands exponentially
- algorithms struggle (especially distance-based methods)

### What actually happens?
- **Distance to corners increases** with dimension (most points end up near faces/surface).
- **Distance concentration:** points appear almost equally far.
- **High-dimensional random vectors become almost orthogonal** → similarity measures lose meaning.

### Hypercube vs Hypersphere intuition
- **Hypercube:** all possible combinations of feature values (representation space)
- **Hypersphere:** where real data actually lies
- With increasing dimensions: hypersphere occupies smaller fraction of cube → most space is empty.

---

## Data Requirement Explosion
Coverage grows exponentially:
- 1D density = 5 points
- 2D = 5² = 25
- 3D = 5³ = 125

So, high dimensions require **exponentially more data**.

---

## How to deal with Curse of Dimensionality
### Feature Extraction
- [[PCA]]
- [[LDA]]
- ICA
- t-SNE
- Isomap
- Autoencoders (manifold learning)

### Feature Selection
Choose subset of features (remove irrelevant ones).


