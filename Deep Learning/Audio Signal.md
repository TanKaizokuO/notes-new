---
tags:
  - extension
---
---
## Definition
An **audio signal** is a representation of sound that encodes all information needed to reproduce sound.

---

## Analog vs Digital Signals

### Analog Signal
* **Continuous time**: Defined at every instant.
* **Continuous amplitude**: Can take any value within a range.

### Digital Signal
* **Discrete time samples**: Defined only at specific intervals.
* **Finite amplitude levels**: Values are snapped to a specific grid.

---

# Analog-to-Digital Conversion (ADC)
ADC converts continuous analog signals into digital data through two main steps:
1.  **Sampling** (Time discretization)
2.  **Quantization** (Amplitude discretization)

**Output**:
The result is often called an **Audio Digital Signal** or **PCM (Pulse Code Modulation)**.

---

## Sampling
Sampling involves taking measurements of the signal at regular time intervals.
* **Sampling Period ($T$)**: The time between samples.
* **Sampling Rate ($S_r$)**: Samples per second (Hz).

$$S_r = \frac{1}{T}$$

### Sampling Error
The difference between the original continuous waveform area and the sampled approximation.
* **Lower Sampling Rate** $\to$ Higher Sampling Error $\to$ Information Loss.

---

## Nyquist Frequency and Aliasing
To perfectly reconstruct a signal, we must adhere to the **Nyquist-Shannon Sampling Theorem**.



**Nyquist Frequency ($f_N$):**
$$f_N = \frac{S_r}{2}$$

**Condition to Avoid Aliasing:**
The sampling rate must be at least twice the maximum frequency present in the signal.
$$S_r \ge 2 f_{max}$$

> [!WARNING] Aliasing
> If $S_r < 2 f_{max}$, high-frequency signals are indistinguishable from low-frequency signals. They "fold over" and appear as lower frequency noise.

---

## Quantization
Quantization converts the sampled continuous amplitudes into discrete levels.



* **Bit Depth ($n$)**: Determines the number of available levels ($2^n$).
    * *Example (CD Quality)*: 16-bit $\to$ 65,536 levels.
* **Resolution**: Higher bit depth $\to$ finer resolution.

### Quantization Error
The difference between the actual amplitude $x(n)$ and the rounded digital value $Q(x(n))$.
$$e_q = x(n) - Q(x(n))$$

---

## Dynamic Range and SQNR

### Dynamic Range (DR)
The ratio between the loudest (max) and quietest (min) detectable parts of the signal.
$$DR = 20\log_{10}\left(\frac{A_{max}}{A_{min}}\right)$$

### Signal-to-Quantization-Noise Ratio (SQNR)
SQNR relates the maximum signal strength to the noise introduced by quantization error. A handy rule of thumb is:

$$SQNR \approx 6.02n \text{ dB}$$

**For 16-bit Audio:**
$$SQNR(16) \approx 6.02(16) \approx 96 \text{ dB}$$

> [!NOTE] Conclusion
> **Higher Bit Depth** $\to$ More Levels $\to$ Less Quantization Noise $\to$ Higher Dynamic Range.

---

## Related Notes
* [[Audio Representation]]
* [[Spectrogram]]