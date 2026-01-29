---
tags:
  - main
---
---

# LeNet-5 (1998)

![[lenet.webp]]

**Purpose:** Designed by Yann LeCun for handwritten digit recognition (MNIST dataset). It effectively pioneered the use of Convolutional Neural Networks for practical applications (like reading checks).

---

## 1. Input Layer

- **Dimensions:** $32 \times 32 \times 1$
    
- **Explanation:** Accepts a grayscale image.
    
- **Note:** While MNIST images are $28 \times 28$, LeNet-5 padded them to $32 \times 32$. This ensured that distinctive features (like the stroke of a '7' or '2') appeared in the center of the receptive fields of the highest-level feature detectors.
    

## 2. Feature Extraction (Convolution & Subsampling)

The architecture follows a strict pattern of `Convolution` $\to$ `Subsampling` (Pooling).

### Layer C1 (Convolution)

- **Operation:** Convolves 6 filters of size $5 \times 5$ over the input.
    
- **Output (Feature Map):** $28 \times 28 \times 6$
    
    - _Calculation:_ $32 - 5 + 1 = 28$.
        
- **Insight:** Extracts basic features like edges and corners. The depth increases from 1 (grayscale) to 6 feature maps.
    

### Layer S2 (Subsampling)

- **Operation:** Reduces the spatial resolution. In the original LeNet-5, this was **Average Pooling** (taking the average of the $2 \times 2$ window, multiplying by a trainable coefficient, and adding a bias). Modern implementations often substitute this with Max Pooling.
    
- **Output:** $14 \times 14 \times 6$
    
- **Insight:** Reduces the dimensionality of the feature maps while retaining the most important spatial information, making the network invariant to small shifts and distortions.
    

### Layer C3 (Convolution)

- **Operation:** Convolves 16 filters of size $5 \times 5$.
    
- **Output:** $10 \times 10 \times 16$
    
    - _Calculation:_ $14 - 5 + 1 = 10$.
        
- **Insight:** This layer increases the network depth ($6 \to 16$). In the original paper, the connection table here was **sparse** (not every input channel connected to every output filter) to force the network to learn global features and break symmetry.
    

### Layer S4 (Subsampling)

- **Operation:** Similar to S2, this layer performs subsampling (pooling).
    
- **Output:** $5 \times 5 \times 16$
    
- **Insight:** The spatial resolution is now very low ($5 \times 5$), meaning the exact position of the features matters less than their relative position to one another.
    

---

## 3. Classification Layers

The final stage flattens the 2D feature maps into 1D vectors for classification.

### Layer C5 (Convolution / Fully Connected)

- **Dimensions:** 120 Units
    
- **Operation:** This is labeled as a "Convolution" in the diagram because it uses $5 \times 5$ filters. However, since the input size is exactly $5 \times 5$, the filter covers the entire input volume.
    
- **Equivalence:** Mathematically, this is identical to a **Fully Connected (Dense) layer** where every input node ($5 \times 5 \times 16 = 400$) connects to every output node (120).
    

### Output Layers (F6 & Output)

- **F6 (Not shown in diagram, standard in LeNet):** Usually connects the 120 units to 84 units (specifically chosen to match the ASCII bitmap target of that era).
    
- **Output:** Finally connects to 10 units (one for each digit 0-9) with a **Softmax** activation (originally Radial Basis Function or RBF) to output probabilities.
    

---

## Why Important

1. **Spatial Hierarchies:** It proved that learning features directly from raw pixels (using convolutions) was superior to hand-crafted feature extraction.
    
2. The "Standard" Recipe: It established the standard Deep Learning recipe used for decades:
    
    $$Convolution \rightarrow Nonlinearity \rightarrow Pooling \rightarrow Dense$$
    
3. **Parameter Efficiency:** By sharing weights (convolutions), it used far fewer parameters than a standard multi-layer perceptron (MLP) would for the same image size.
    

---

## Summary of Metrics

- **Total Parameters:** $\approx 60,000$ (Tiny by modern standards).
    
- **Input Size:** $32 \times 32$ pixels.
    
- **Total Layers:** 7 (Input $\to$ C1 $\to$ S2 $\to$ C3 $\to$ S4 $\to$ C5 $\to$ F6 $\to$ Output).