---
tags:
  - main
---
---
# Residual Networks (ResNet)

![[ResNet.png]]

ResNet introduced [[Residual Block (Skip Connection)]] to train very deep neural networks effectively.

---
## Why ResNet Works

### ✅ 1) Easier optimization
Instead of learning the full mapping $H(x)$, the network learns the residual:
$$F(x) = H(x) - x$$

If the identity mapping is optimal, it is easier to learn $F(x)=0$ than to learn $H(x)=x$ from scratch.

### ✅ 2) Better gradient flow
$$\frac{\partial (F(x)+x)}{\partial x} = \frac{\partial F(x)}{\partial x} + 1$$

The $+1$ term allows gradients to flow directly back through the network, preventing the vanishing gradient problem.

### ✅ 3) Enables very deep networks
Examples: ResNet-50, ResNet-101, ResNet-152.