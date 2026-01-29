---
tags:
  - main
---
---
# Audio Representation

## What is Sound?
Sound is produced by the **vibration** of an object.
1.  **Vibration** causes air molecules to oscillate.
2.  **Oscillations** create changes in air pressure.
3.  **Pressure change** propagates as a **sound wave**.

> [!INFO] Physics Definition
> Sound is a **mechanical wave** that requires a medium (solid, liquid, or gas) to travel.

---

## Waveform
Sound can be represented as a **complex waveform**. A waveform carries three main attributes:
* **Frequency** (Physical) $\to$ **Pitch** (Perceptual)
* **Intensity / Amplitude** (Physical) $\to$ **Loudness** (Perceptual)
* **Timbre** (Spectral Content) $\to$ **Tone Color** (Perceptual)



### Periodic vs. Aperiodic Sound
* **Periodic**: Repeats over time.
    * *Simple*: Single sine wave.
    * *Complex*: Sum of multiple sinusoids (e.g., musical instruments).
* **Aperiodic**: Does not repeat.
    * *Continuous*: Noise (white noise, pink noise).
    * *Transient*: Short pulse (click, snare hit).

---

## Sinusoidal Representation
A pure tone (sine wave) can be mathematically defined as:

$$y(t) = A \sin(2\pi f t + \phi)$$

Where:
* $A$: Amplitude
* $f$: Frequency
* $t$: Time
* $\phi$: Phase

---

## Pitch (Perception of Frequency)
Pitch is the **subjective perception** of frequency.
* **Range**: Humans typically hear **~20 Hz to 20 kHz** (decreases with age).
* **Logarithmic**: Our perception is not linear. Two frequencies sound "similar" if they differ by powers of 2 (Octaves).

### Pitch-to-Frequency Formula
For standard MIDI tuning (where A4 = 440Hz is note 69):

$$F(p) = 2^{\frac{p-69}{12}} \cdot 440$$

**The 12th Root of 2:**
To move up one semitone, the frequency is multiplied by:
$$\frac{F(p+1)}{F(p)} = 2^{1/12} \approx 1.059$$

---

## Intensity & Loudness

### 1. Sound Power & Intensity
* **Sound Power**: Energy transferred per unit time. Measured in **Watts (W)**.
* **Sound Intensity**: Sound power per unit area. Measured in **W/m²**.

### 2. Intensity Level (Decibels)
Since the range of human hearing is vast, we use a logarithmic scale (Decibels).

$$dB(I) = 10 \log_{10}\left(\frac{I}{I_{TOH}}\right)$$

* $I_{TOH}$: **Threshold of Hearing** $\approx 10^{-12} \, W/m^2$.

### 3. Loudness
Loudness is the subjective perception of intensity, measured in **phons**. It depends on:
* Duration
* Frequency (Fletcher-Munson curves)
* Age

---

## Timbre (Tone Color)
Timbre is what allows us to distinguish two sounds that have the same **Pitch**, **Loudness**, and **Duration**.
* *Example:* Distinguishing a Trumpet vs. Violin playing A440.
* *Descriptors:* Bright, dark, dull, harsh, warm.

**Dimensions of Timbre:**
1.  Sound Envelope (ADSR)
2.  Harmonic Content (Spectrum)
3.  Modulation (Vibrato/Tremolo)

---

## Sound Envelope (ADSR)
The envelope describes how the amplitude varies over time.



* **Attack**: How quickly the sound reaches peak amplitude.
* **Decay**: The drop from peak to the sustain level.
* **Sustain**: The constant amplitude while the note is held.
* **Release**: How quickly the sound fades after the note is stopped.

> [!NOTE] Related Notes
> See: [[Audio Features]], [[Time Domain Audio Features|Amplitude Envelope]]

---

## Complex Sound = Sum of Sinusoids
According to Fourier theory, any complex periodic sound is a superposition of sinusoids.
* **Partial**: Each individual sinusoid component.
* **Fundamental Frequency ($f_0$)**: The lowest partial; determines the perceived pitch.
* **Harmonics**: Partials that are integer multiples of the fundamental ($2f_0, 3f_0, \dots$).

**Inharmonicity:**
* *Pitched instruments* (Guitar, Violin) $\to$ Mostly harmonic.
* *Percussion* (Cymbals, Bells) $\to$ Inharmonic (partials are not integer multiples).

---

## Modulation
* **Frequency Modulation (Vibrato)**: Periodic variation in frequency.
* **Amplitude Modulation (Tremolo)**: Periodic variation in amplitude.

---

## Related Notes
* [[Audio Signal]]
* [[Audio Features]]
* [[Spectrogram]]