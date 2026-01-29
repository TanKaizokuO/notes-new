---
tags:
  - main
---
---

Instead of over-planning early, an usefull [[DL Strategy]] would be to build a working system fast and improve iteratively.

---

## Example: Speech Recognition

**Possible error sources (The "kitchen sink" list):**
- Noisy background
- Café noise
- Car noise
- Accented speech
- Far from microphone
- Children’s speech
- Stuttering

**The Trap**: Spending months designing a system to handle *all* of these before building anything.

---

## Recommended Process

1. **Setup**: Set up **Dev/Test sets** and a clear **evaluation metric**.
2. **Build**: Build an initial system quickly (e.g., in a few days).
3. **Analyze**: Use the initial system to get performance data.
    - Use **Bias/Variance analysis**.
    - Use **Error analysis**.
4. **Iterate**: Use insights to prioritize specific improvements (e.g., "only car noise is actually hurting us").