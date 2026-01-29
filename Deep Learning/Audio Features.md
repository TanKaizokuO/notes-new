---
tags:
  - extension
---
---
## Overview
Audio features are descriptors of sound. Different features capture different aspects of the signal and are the building blocks for intelligent audio systems.

---

## Categories of Audio Features
Audio features are generally categorized by:
1.  **Level of abstraction**
2.  **Temporal scope**
3.  **Music aspect**
4.  **Signal domain**
5.  **ML approach**

---

## Level of Abstraction



### 1. Low-level Features
* **Description**: Makes sense to machines; not directly meaningful to humans.
* **Examples**:
    * Amplitude Envelope
    * Energy
    * Spectral Centroid
    * Spectral Flux
    * Zero Crossing Rate (ZCR)

### 2. Mid-level Features
* **Description**: Perceptual features often related to musical structures.
* **Examples**:
    * Pitch
    * Beat-related descriptors
    * Onset patterns
    * MFCCs (Mel-frequency cepstral coefficients)

### 3. High-level Features
* **Description**: Human-friendly concepts; often semantic.
* **Examples**:
    * Instrumentation, Key, Chord progression
    * Rhythm, Tempo, Lyrics
    * Genre, Mood

---

## Temporal Scope
* **Instantaneous**: Computed per short frame ($\approx 50\text{ms}$).
* **Segment-level**: Computed over larger blocks (seconds).
* **Global**: Computed over the entire audio file (aggregates like mean, median, or GMMs).

---

## Signal Domain
Features are often classified by the domain from which they are extracted.

### Time Domain
Extracted directly from the raw waveform samples.
* [[Time Domain Audio Features|Amplitude Envelope]]
* [[Time Domain Audio Features|RMS Energy]]
* [[Time Domain Audio Features|Zero Crossing Rate]]

### Frequency Domain
Extracted after a Fourier Transform (FT).
* Band Energy Ratio
* Spectral Centroid
* Spectral Flux
* Spectral Roll-off
* Spectral Spread

### Time-Frequency Representation
Features that capture both frequency content and temporal evolution.
* [[Spectrogram]]
* [[Mel-Spectrogram]]
* Constant-Q Transform

---

## ML Perspective (Feature Usage)
* **Traditional ML**: Relies on "hand-crafted" features (Amplitude envelope, RMS, ZCR, Spectral Centroid/Flux/Spread).
* **Deep Learning**: Relies on raw data or 2D representations ([[Spectrogram]], [[Mel-Spectrogram]]).

---

# Framing Pipeline (Core Preprocessing)
Feature extraction rarely happens on the raw continuous signal; it works on **frames**.



## Frames
Frames are short, perceivable audio chunks.
* **Size**: Typically a power of 2 ($256$ to $8192$ samples) for Fast Fourier Transform (FFT) efficiency.
* **Ear Resolution**: The human ear has a time resolution of $\approx 10\text{ms}$.
* **Sample Duration**: At $44.1\text{kHz}$, 1 sample is $0.0227\text{ms}$ (much smaller than ear resolution), hence grouping into frames is necessary.

## Spectral Leakage
Leakage occurs when the signal within a frame does not complete an integer number of cycles. This results in discontinuities at the frame edges, which introduces artificial frequency components (noise).



**Fix**: Apply Windowing].

## Windowing
We apply a window function $w(k)$ to each frame to taper the edges to zero.

$$s_w(k) = s(k) \cdot w(k)$$

* **Effect**: Reduces discontinuities at endpoints $\to$ Reduces spectral leakage.
* **Common Window**: Hann Window.

## Overlapping Frames
Since windowing dampens the signal at the edges, we lose information there. To fix this, frames are overlapped (usually by 50%).
* **Benefits**:
    * Restores information lost at edges.
    * Introduces redundancy.
    * Improves time-frequency resolution.

---

## Related Notes
* [[Audio Representation]]
* [[Audio Signal]]
* [[Spectrogram]]