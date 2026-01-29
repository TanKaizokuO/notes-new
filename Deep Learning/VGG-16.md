---
tags:
  - main
---

---

# VGG-16 (2014)

![[VGG-16.jpg]]

**Purpose:** Improved ImageNet performance by pushing the depth of the network (16 weight layers) using a very uniform architecture.

---

## 1. Input Layer

- **Dimensions:** $224 \times 224 \times 3$
    
- **Explanation:** The network accepts a standard RGB image. The diagram shows the input image (rally car) being fed into the first stack of convolutional layers. Mean pixel subtraction is usually applied here as a preprocessing step.
    

## 2. Convolutional Blocks (Feature Extraction)

The VGG-16 architecture is distinct for its uniform pattern: stacks of Convolutional layers followed by a Max Pooling layer. The diagram highlights 5 distinct "blocks" where the image size shrinks while the depth (number of filters) increases.

### Block 1

- **Structure:** 2 $\times$ Convolutional Layers ($64$ filters) + ReLU.
- **Output:** $224 \times 224 \times 64$
- **Pooling:** Followed by Max Pooling (purple block) which halves the size to $112 \times 112$.
- **Insight:** Unlike AlexNet which started with a large $11 \times 11$ kernel, VGG starts immediately with small $3 \times 3$ kernels. This preserves fine spatial details early on.

### Block 2

- **Structure:** 2 $\times$ Convolutional Layers ($128$ filters) + ReLU.
- **Output:** $112 \times 112 \times 128$
- **Pooling:** Max Pooling halves the size to $56 \times 56$.
- **Insight:** The number of filters doubles (64 $\to$ 128), allowing the network to learn more complex textures.

### Block 3

- **Structure:** 3 $\times$ Convolutional Layers ($256$ filters) + ReLU.
- **Output:** $56 \times 56 \times 256$
- **Pooling:** Max Pooling halves the size to $28 \times 28$.
- **Insight:** This is where the network gets deeper (3 layers instead of 2). This added non-linearity allows it to distinguish specific object parts (e.g., wheels, windows).

### Blocks 4 & 5

- **Structure:** 3 $\times$ Convolutional Layers ($512$ filters) + ReLU in each block.
- **Output (Block 4):** $28 \times 28 \times 512$ $\xrightarrow{pool}$ $14 \times 14$.
- **Output (Block 5):** $14 \times 14 \times 512$ $\xrightarrow{pool}$ $7 \times 7$.    
- **Insight:** The spatial dimension is now very small ($7 \times 7$), but the depth is significant ($512$ channels). This represents a high-level "semantic" understanding of the image content.

---

## 3. Fully Connected Layers (Classification)

The final green blocks in the diagram represent the dense layers that perform the actual classification.

- **FC1 & FC2:**
    
    - **Dimensions:** $1 \times 1 \times 4096$
    - **Operation:** The $7 \times 7 \times 512$ volume is flattened and passed through two massive fully connected layers with 4,096 neurons each.
    - **Note:** These layers contain the vast majority of the network's parameters ($\approx 100$ million+), making the model heavy to store (500MB+).
    
- **FC3 (Output):**
    - **Dimensions:** $1 \times 1 \times 1000$
    - **Operation:** Softmax activation (red block).
    - **Result:** A probability distribution over the 1,000 ImageNet classes.

---

## Why $3 \times 3$ Convs are Powerful (The "VGG Choice")

The diagram emphasizes the repeated use of standard green convolution blocks. This was a deliberate design choice over larger filters.

1. **Receptive Field Equivalence:**
    
    - Stacking **two** $3 \times 3$ layers creates an effective receptive field of $5 \times 5$.
        
    - Stacking **three** $3 \times 3$ layers creates an effective receptive field of $7 \times 7$.
        
2. **Parameter Efficiency:**
    
    - Three $3 \times 3$ filters have $3 \times (3^2 \times C^2) = 27C^2$ weights.
        
    - One single $7 \times 7$ filter has $1 \times (7^2 \times C^2) = 49C^2$ weights.
        
    - _Result:_ VGG captures the same spatial context as a large filter but with **45% fewer parameters**.
3. **More Non-Linearity:**  Using three stacked layers means three ReLU activations instead of just one. This makes the decision function more discriminative.

---

## Summary of Metrics

- **Total Parameters:** $\approx 138$ Million.
- **Architecture Depth:** 16 learnable layers (13 Convolutional + 3 Fully Connected).    
- **Tradeoff:** Excellent accuracy for its time, but very slow to train and expensive to run inference due to the massive Fully Connected layers.

---

Would you like me to generate a comparison table between AlexNet and VGG-16 to add to your notes?