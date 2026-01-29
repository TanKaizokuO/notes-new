---
tags:
  - extension
---
---

Time domain features are extracted directly from the raw audio waveform samples. They are computationally efficient and provide fundamental information about loudness and noisiness.

---

## 1. Amplitude Envelope (AE)
The Amplitude Envelope tracks the shape of the sound's volume over time.

**Definition:**
* It is the **maximum amplitude value** of all samples within a frame.
* Serves as a rough indicator of loudness.

**Drawback:**
* Highly **sensitive to outliers** (a single loud spike dictates the frame's value).

**Applications:**
* **Onset Detection**: Identifying the start of a note or word.
* **Genre Classification**.



---

## 2. RMS Energy (Root Mean Square)
RMS Energy provides a more robust measure of loudness than the Amplitude Envelope.

**Definition:**
* The Root Mean Square of all samples in a frame.
* Since $\text{Amplitude}^2 \propto \text{Energy}$, RMS is a reliable indicator of perceived loudness.
* **Less sensitive to outliers** than AE because it averages the energy across the frame.

**Formula:**
$$RMS_t = \sqrt{\frac{1}{K}\sum_{k=1}^{K} s(k)^2}$$
*(Where $K$ is the number of samples in the frame)*

**Applications:**
* **Audio Segmentation**: Detecting silent vs. active regions.
* **Music Genre Classification**.

---

## 3. Zero Crossing Rate (ZCR)
ZCR measures how rapidly the signal changes sign (positive to negative or vice versa).

**Definition:**
* The number of times the signal crosses the horizontal axis (zero amplitude) within a frame.
* It is a proxy for the "noisiness" or frequency content of a signal.

**Interpretation:**
* **High ZCR**: Noisy, percussive, or unvoiced sounds (e.g., snare drum, 's' sound).
* **Low ZCR**: Tonal, pitched, or voiced sounds (e.g., violin, vowels).



**Formula:**
$$ZCR_t = \frac{1}{2}\sum_{k=1}^{K-1} |\text{sgn}(s(k)) - \text{sgn}(s(k+1))|$$

**Applications:**
* **Speech Recognition**: Distinguishing voiced vs. unvoiced speech.
* **Music Information Retrieval**: Distinguishing percussion from melody.

---

**Backlinks:** [[Audio Features]]