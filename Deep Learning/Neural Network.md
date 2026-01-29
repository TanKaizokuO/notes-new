---
tags:
  - main
---
---
### Notation

- **$m$**: training examples
- **$n_x$**: length of $x^{(i)}$
- $X$: Matrix of input features
  $$X = [x^{(1)}, x^{(2)}, \dots, x^{(m)}]$$
  - **Shape**: $(n_x, m)$

- $Y$: Matrix of labels
  $$Y = [y^{(1)}, y^{(2)}, \dots, y^{(m)}]$$
  - **Shape**: $(1, m)$


### Logistic Regression

When we consider logistic regression:

- **Output**: $\hat{y} = \sigma(w^T x + b)$
    - Where $\sigma(z)$ is the sigmoid function:
        $$\sigma(z) = \frac{1}{1 + e^{-z}}$$


### Functions

- **Loss Function**: Used for a single training example.
    $$L(\hat{y}, y) = -(y \log(\hat{y}) + (1-y) \log(1-\hat{y}))$$
    
- **Cost Function**: The average of the loss functions across the entire training set.
    $$J(w,b) = \frac{1}{m} \sum_{i=1}^{m} L(\hat{y}^{(i)}, y^{(i)})$$

**Optimization**: We use **gradient descent** to minimize $J(w,b)$. Note that $J$ is a **convex function**.

---

## Vectorization

**Formula**: $Z = w^T X + b$

### Comparison

**Non-vectorized approach**:

```python
Z = 0
for i in range(n_x):
    Z += w[i] * x[i]
Z += b
````

**Vectorized approach**:

Python

```
Z = np.dot(w, x) + b
```

> [!INFO] Benefits
> 
> Vectorization decreases execution time by helping us avoid explicit loops.

---

## Broadcasting in Python

**Vectorized Logistic Regression**: `Z = np.dot(w.T, X) + b`

### Example: Calculating Percentage of Calories

Given a matrix of nutrition data (Protein, Carbs, Fat):

$$A = \begin{bmatrix} 5.6 & 0.0 & 4.4 & 68.0 \\ 1.2 & 104.0 & 52.0 & 8.0 \\ 1.8 & 135.0 & 99.0 & 0.9 \end{bmatrix}$$

**Goal**: Find the % of calories from Protein, Carbs, and Fat for each column.

Python

```PYTHON
import numpy as np

# Defining the array
A = np.array([
    [5.6, 0.0, 4.4, 68.0],
    [1.2, 104.0, 52.0, 8.0],
    [1.8, 135.0, 99.0, 0.9]
])

# Sum vertically (axis=0)
cal = A.sum(axis=0) 
# Result: [59, 239, 154.4, 76.9]

# Calculate percentage using broadcasting
# Reshape is technically redundant here (rank 1 array behaves nicely) but considered good practice for clarity
percent = 100 * A / cal.reshape(1, 4)
```

### How Broadcasting Works

Python automatically expands the smaller array to match the dimensions of the larger array.

**Simple Example:**

If $A$ is a $(2, 3)$ matrix and $B$ is a $(1, 3)$ matrix:

$$A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}, \quad B = \begin{bmatrix} 1 & 2 & 3 \end{bmatrix}$$

When computing $A + B$, Python will copy $B$ to match the rows of $A$:

$$B \to \begin{bmatrix} 1 & 2 & 3 \\ 1 & 2 & 3 \end{bmatrix}$$

### Key Changes Made:
1.  **Code Blocks**: Changed raw text blocks to standard Markdown code fences (` ```python `) for syntax highlighting.
2.  **LaTeX Spacing**: Added newlines before and after `$$` blocks. In the original text, lines like `$$X = ...$$- **Shape**` were mashed together, which prevents Obsidian from rendering the math correctly.
3.  **Callout Format**: Cleaned up the `> [!INFO]` block to ensure it renders as a proper callout box.
4.  **List Hierarchy**: Fixed indentation for the definitions of Shape and Sigmoid to make them visually subordinate to their parent bullets.