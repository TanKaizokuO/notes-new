---
tags:
  - extension
---
---

This technique is used to smooth out noisy data sequences (like daily temperature readings or loss during training) to reveal the underlying trend. It works by calculating a running average where more recent observations are given higher weight, and older observations are given exponentially decreasing weight.

![[Selection_003.png]]
### Visual Intuition

In the image provided:

- **Blue Dots:** Represent the raw, noisy data (input sequence $t$).
    
- **Red Line ($\beta \approx 0.9$):** Represents a moving average over approx. 10 days. It tracks the data closely but is still somewhat "jittery."
    
- **Green Line ($\beta \approx 0.98$):** Represents a moving average over approx. 50 days. This line is much **smoother** because it is less sensitive to individual outliers, but it is also **slower to adapt** (shifted to the right) because it gives more weight to the past history.
    

---

### Mathematical Formulation

**Given:** A sequence $t_1, t_2, t_3, ..., t_n$

**Initialize:** $V_0 = 0$

Recursive Update Rule:

$$V_t = \beta V_{t-1} + (1-\beta)t_t$$

Where:

- $V_t$: The estimated moving average at time step $t$.
    
- $\beta$: The hyperparameter controlling the "window" size.
    
    - $\frac{1}{1-\beta}$ approximates the number of days effectively averaged over.
        
    - Example: If $\beta = 0.9$, then $\frac{1}{1-0.9} = 10$ days.
        
- $t_t$: The actual observation at time step $t$.
    

Example Step-by-Step:

$$V_1 = 0.9V_0 + 0.1t_1$$

$$V_2 = 0.9V_1 + 0.1t_2$$

$$\dots$$

$$V_n = 0.9V_{n-1} + 0.1t_n$$

---

### Bias Correction

The Problem:

Since we initialize $V_0 = 0$, the early estimates ($V_1, V_2, ...$) are artificially low (biased toward 0).

- _Visual Evidence:_ Notice in the image where the **purple line** starts near 0 on the Y-axis (indicated by the blue arrow). This is the "warm-up" phase where the average is inaccurate.
    

The Solution:

To compensate for this initial dampening, we divide by a correction factor:

$$\text{Corrected } V_t = \frac{V_t}{1-\beta^t}$$

- When $t$ is small, $1-\beta^t$ is a decimal $< 1$, which scales $V_t$ up.
    
- As $t$ increases, $\beta^t \to 0$, so the denominator $\to 1$, and bias correction naturally turns off once the average stabilizes.