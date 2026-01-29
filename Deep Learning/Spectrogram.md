---
tags:
  - extension
---
---

## Definition
A **spectrogram** is a visual representation of the spectrum of frequencies of a signal as it varies with time.

* **X-axis**: Time (Frames)
* **Y-axis**: Frequency (Hz or Mel)
* **Color/Intensity**: Magnitude or power of the frequency content (often in dB).



---

## Why STFT? (The Problem with DFT)
Standard Discrete Fourier Transform (DFT) works on the full signal.
* **DFT Output**: Tells you *what* frequencies are present.
* **Missing Info**: It does not tell you *when* those frequencies occurred.

> [!NOTE] The Goal
> We need a time-frequency representation. We achieve this by breaking the signal into small chunks and analyzing each chunk separately.

---

# STFT (Short-Time Fourier Transform)

## The Core Idea
Instead of applying DFT to the entire signal at once:
1.  **Frame**: Split the signal into short segments.
2.  **Window**: Apply a window function to smooth edges.
3.  **DFT**: Apply DFT to each frame.
4.  **Stack**: Stack the results to form a matrix.



## Windowing & Overlap
To prevent spectral leakage and data loss at frame edges, we use:
* **Window Function ($w$)**: Typically a **Hann window**.
    $$x_w(k) = x(k) \cdot w(k)$$
* **Hop Length ($H$)**: The number of samples we shift to get the next frame.
    * *Common*: $H = N/2$ or $N/4$ (50% or 75% overlap).

## STFT Formula
$$S(m,k) = \sum_{n=0}^{N-1} x(n+mH) \cdot w(n) \cdot e^{-i2\pi nk/N}$$

* $x(n)$: Input signal
* $w(n)$: Window function
* $N$: Window/Frame length
* $H$: Hop size
* $m$: Frame index (Time)
* $k$: Frequency bin index

---

## The Output (Dimensions)
The result is a complex 2D matrix (Magnitude + Phase).

* **Number of Frequency Bins**:
    $$\text{# bins} = \frac{\text{Frame Size}}{2} + 1$$
    *(Due to Nyquist symmetry)*
* **Number of Time Frames**:
    $$\text{# frames} = \frac{\text{Total Samples} - \text{Frame Size}}{\text{Hop Size}} + 1$$

---

## The Time-Frequency Trade-off
You cannot have perfect resolution in both time and frequency simultaneously (Heisenberg Uncertainty Principle).

> [!WARNING] The Trade-off
> * **Larger Frame Size ($N$)**:
>     * Good Frequency Resolution (more bins).
>     * Poor Time Resolution (smears short events).
> * **Smaller Frame Size ($N$)**:
>     * Good Time Resolution.
>     * Poor Frequency Resolution.

**Relationship:**
$$\Delta t = \frac{N}{f_s}, \quad \Delta f = \frac{f_s}{N}$$

---

# Mel-Spectrogram

The **Mel-Spectrogram** is the standard input for modern Deep Learning audio models (like CNNs).
* **Y-axis**: Mel scale (Perceptually relevant frequency).
* **Value**: Decibels (Perceptually relevant amplitude).

## The Mel Scale
Humans perceive frequency logarithmically. We can distinguish low frequencies much better than high frequencies.
* **< 1kHz**: Perception is roughly linear.
* **> 1kHz**: Requires larger Hz gaps to perceive the same "pitch" difference.



**Formulas:**
$$m = 2595 \log_{10}\left(1 + \frac{f}{700}\right)$$
$$f = 700 (10^{m/2595} - 1)$$
*(Note: Constants may vary slightly by implementation, e.g., 700 vs 500 Hz).*

---

## Mel Filter Banks
To convert a standard Spectrogram to a Mel-Spectrogram, we apply a **Filter Bank**.

1.  **Create Filters**: Generate triangular filters spaced according to the Mel scale.
    
2.  **Matrix Multiplication**:
    $$\text{MelSpec} = M \times Y$$
    * $M$: Filter Bank Matrix `(n_mels, freq_bins)`
    * $Y$: Linear Spectrogram `(freq_bins, n_frames)`

---

## Applications
* Audio Classification
* Mood/Emotion Recognition
* Music Genre Classification
* Instrument Identification

---

## Related Notes
* [[Audio Features]]
* [[Audio Signal]]
* [[Audio Representation]]