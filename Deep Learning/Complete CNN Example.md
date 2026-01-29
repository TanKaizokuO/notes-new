---
tags:
  - main
---
---
![[IMG_0993.png]]
# Digit Recognition Example

A classic architecture style (similar to LeNet-5) for digit classification.


---
## Input
$$32 \times 32 \times 3$$

---

## Layer 1

### Conv1
- $f=5, s=1$
- **Output:** $28 \times 28 \times 6$

### Pool1 (MaxPool)
- $f=2, s=2$
- **Output:** $14 \times 14 \times 6$

---

## Layer 2

### Conv2
- $f=5, s=1$
- **Output:** $10 \times 10 \times 16$

### Pool2
- $f=2, s=2$
- **Output:** $5 \times 5 \times 16$

**Flatten:**
$$5 \times 5 \times 16 = 400$$

---

## Fully Connected Layers

### FC3
$$(400) \rightarrow (120)$$

### FC4
$$(120) \rightarrow (84)$$

---

## Output Layer

**Softmax layer:**
$$10 \text{ outputs}$$

---

## Why Convolutions?

Key reasons why CNNs work better than standard NN for images:
1.  **Parameter Sharing**: A feature detector (e.g., vertical edge) useful in one part of the image is likely useful in another.
2.  **Sparsity of Connections**: In each layer, each output value depends on only a small number of inputs.
3.  **Translation Invariance**: The network becomes robust to objects shifting position in the image (especially with pooling).