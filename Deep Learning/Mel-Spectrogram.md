---
tags:
  - extension
---
---

## Definition
A **Mel-spectrogram** is a time–frequency representation of audio where:
* The **Frequency axis** is mapped to the **Mel scale** (mimicking human pitch perception).
* The **Magnitude** is represented in the **dB scale** (mimicking human loudness perception).
* It is the standard input feature for modern **Audio ML** models (CNNs, RNNs, Transformers).



---

## Why Mel-Spectrogram?
Standard spectrograms use **linear frequency spacing**. However, human hearing is **logarithmic**:
* We are highly sensitive to small changes in low frequencies.
* We need large changes in high frequencies to perceive the same "pitch distance."

> [!SUCCESS] The Solution
> The Mel-spectrogram solves this by grouping frequencies into **Mel-scaled bins**, giving more resolution to low frequencies and less to high frequencies, matching our biology.

---

## What it Represents
* **X-axis**: Time (Frames)
* **Y-axis**: Mel Frequency Bands (Low $\to$ High)
* **Intensity/Color**: Energy/Magnitude (dB)

**Summary**: It shows *how energy is distributed across perception-friendly frequency bands over time*.

---

## Pipeline to Compute Mel-Spectrogram

### Step 1: Framing
Split the continuous signal into short overlapping frames.
* **Frame Size ($N$)**: E.g., 1024, 2048 samples.
* **Hop Size ($H$)**: E.g., 256, 512 samples.

### Step 2: Windowing
Apply a window function $w(n)$ (typically **Hann**) to each frame.
$$x_w(n) = x(n) \cdot w(n)$$

**Purpose:**
* Reduce discontinuities at frame edges.
* Reduce **spectral leakage**.

### Step 3: STFT
Compute the **Short-Time Fourier Transform** to get the frequency spectrum for each frame.

$$X(m,k) = \sum_{n=0}^{N-1} x(n+mH) \cdot w(n) \cdot e^{-j2\pi kn/N}$$

**Power Spectrogram:**
We typically use the squared magnitude:
$$|X(m,k)|^2$$

### Step 4: Apply Mel Filter Banks
This is the conversion step. We multiply the linear power spectrum by a **Mel Filter Bank Matrix ($M$)**.

$$S_{mel} = M \cdot |X|^2$$

* **Matrix $M$ Shape**: `(n_mels, n_freq_bins)`
* **Output Shape**: `(n_mels, n_frames)`



> [!NOTE] The Filters
> The rows of $M$ are **triangular filters** that are narrow at low frequencies and get wider as frequency increases. This performs the "grouping" or weighting of the linear bins.

### Step 5: Convert to dB (Log Scale)
Since human loudness perception is logarithmic, we convert the Mel energy values to Decibels.

$$S_{dB} = 10 \log_{10}(S_{mel})$$
*(Note: Use $10\log$ for power, $20\log$ for amplitude).*

---

## Mel Scale Formulas
The mathematical approximation of human pitch perception.

**Hertz $\to$ Mel:**
$$m = 2595 \log_{10}\left(1 + \frac{f}{700}\right)$$

**Mel $\to$ Hertz:**
$$f = 700 \left(10^{m/2595} - 1\right)$$

---
