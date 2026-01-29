---
tags:
  - main
---
---

## Face Verification vs. Recognition

* **Verification (1:1 matching)**:
    * **Input**: Image, Name/ID.
    * **Output**: Whether the input image is that of the claimed person.
* **Recognition (1:K matching)**:
    * **Database**: $K$ persons.
    * **Input**: Image.
    * **Output**: ID if the image is one of the $K$ persons (or "not recognized").
 
---

## One Shot Learning
The **one-shot learning** problem is to recognize a person given just **one single image**.
Standard Softmax classification doesn't work well here because the number of classes is small/changing and training data is scarce.

**Solution**: Learn a **similarity function** $d$:
$$d(\text{img}_1, \text{img}_2) = \text{degree of difference between images}$$

* If $d(\text{img}_1, \text{img}_2)$ is **small** $\rightarrow$ Same person.
* If $d(\text{img}_1, \text{img}_2)$ is **large** $\rightarrow$ Different persons.

---

## Siamese network
A good way to implement a similarity function is to use a **Siamese network**.



* **Mechanism**: Run two identical networks (shared weights) on two different inputs.
* **Encoding**: Focus on the vector computed by a fully connected layer as an **encoding** of the input image $x$ (denoted as $f(x)$).
* **Goal**: Learn parameters so that:
    * $||f(x_i) - f(x_j)||^2$ is **small** for the same person.
    * $||f(x_i) - f(x_j)||^2$ is **large** for different persons.

---

## Triplet Loss
One way to learn the parameters for the Siamese network is to use the **Triplet Loss** function.


You look at three images at once:
* **Anchor (A)**
* **Positive (P)**: Same person as A
* **Negative (N)**: Different person than A

### Learning Objective
We want the distance between Anchor and Positive to be smaller than the distance between Anchor and Negative by at least a margin $\alpha$:
$$d(A, P) + \alpha \le d(A, N)$$

* Where $\alpha$ is a **margin**.

### Loss Function
$$L(A, P, N) = \max(d(A, P) - d(A, N) + \alpha, 0)$$

**Total Cost**:
$$J = \sum L(A^{(i)}, P^{(i)}, N^{(i)})$$

> **Note**: You need a dataset with multiple pictures of the same person. It is important to choose **"hard" triplets** (where $d(A, P) \approx d(A, N)$) to train effectively.

---

## Face Verification and Binary Classification
Face recognition can also be posed as a **binary classification** problem.

* A pair of neural networks (Siamese Network) compute embeddings.
* These embeddings are input to a **Logistic Regression** unit (or a small neural network) to predict:
    * $1$ (Same person)
    * $0$ (Different person)

> **Tip**: Pre-compute encodings for the database images to save computation during inference.

---

## Summary
* **Face Verification** is 1:1; **Face Recognition** is 1:K.
* Triplet Loss is effective for learning face encodings.
* The same encoding is used for both verification and recognition by measuring distances.

---

**Related Topics**:
* [[Verification vs Recognition]]
* [[One-Shot Learning & Similarity Learning]]
* [[Siamese Network]]
* [[Triplet Loss]]
* [[Face Verification as Binary Classification]]
