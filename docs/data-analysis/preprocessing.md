# Data Analysis: Preprocessing

This section describes how to preprocess CSI data collected using the ESP32-CSI Tool to obtain clean, usable phase signals for further analysis. The process includes phase unwrapping and linear fitting based on techniques implemented in the custom RayIoT toolchain.

---

## 1. Why Preprocess Phase Data?

The phase values derived from raw I/Q pairs are wrapped between \(-\pi\) and \(\pi\). These discontinuities affect continuity and trend analysis. Additionally, hardware-induced distortions can bias phase lines. Preprocessing improves phase consistency and removes noise.

---

## 2. Converting I/Q to Amplitude and Phase

Raw CSI data is output as interleaved I (in-phase) and Q (quadrature) samples for each subcarrier. These need to be separated and converted to amplitude and phase:

```python
import numpy as np
from math import sqrt, atan2

# Example: interleaved IQ values from a CSI line
csi_raw = [8, -4, 7, 1, 9, 3, 10, 5]  # I1,Q1, I2,Q2, ...

i_vals = csi_raw[0::2]  # even indices = I
q_vals = csi_raw[1::2]  # odd indices = Q

amplitudes = [sqrt(i**2 + q**2) for i, q in zip(i_vals, q_vals)]
phases = [atan2(q, i) for i, q in zip(i_vals, q_vals)]
```

These amplitude and phase values serve as the foundation for all further analysis.

---

## 3. Phase Unwrapping

Wrapped phase data causes abrupt jumps near \(\pm\pi\). Unwrapping reconstructs continuous phase:

```python
def unwrap_phase(current_phase, next_phase):
    delta = next_phase - current_phase
    if delta > math.pi:
        next_phase = current_phase + delta - (2 * math.pi)
    elif delta < -math.pi:
        next_phase = current_phase + delta + (2 * math.pi)
    return next_phase

# Apply to entire array
for i in range(len(phases) - 1):
    phases[i + 1] = unwrap_phase(phases[i], phases[i + 1])
```

---

## 3. Linear Fitting to Remove Bias

To remove hardware offset trends, a linear fit is applied to the unwrapped phase using the following equation:

$$
\phi_f = \phi_f - (\alpha_1 \cdot f + \alpha_0)
$$

Where:

$$
\alpha_1 = \frac{\phi_F - \phi_1}{2\pi F}, \quad 
\alpha_0 = \frac{1}{F} \sum_{f=1}^{F} \phi_f
$$

### Explanation of Terms

- **$\phi_f$**: The unwrapped phase value at subcarrier index **f** (in radians). This represents the raw phase reading after removing discontinuities.
- **$f$**: Index of the subcarrier, ranging from 1 to F (total number of subcarriers).
- **$\alpha_1$**: Slope of the linear phase trend across subcarriers, typically caused by hardware imperfections.
- **$\alpha_0$**: Offset (intercept) of the phase trend line.

This equation subtracts the best-fit line (trend) from each phase value to flatten the phase response across subcarriers.

---

The slope and offset are computed as:

$$
\alpha_1 = \frac{\phi_F - \phi_1}{2\pi F}, \quad
\alpha_0 = \frac{1}{F} \sum_{f=1}^{F} \phi_f
$$

### Explanation of Terms

- **$\phi_1$**: Phase of the first subcarrier.
- **$\phi_F$**: Phase of the last subcarrier.
- **$F$**: Total number of subcarriers (e.g., 52 for 20 MHz bandwidth).
- **$\alpha_1$**: Approximates the rate of phase change across subcarriers.
- **$\alpha_0$**: Mean phase across all subcarriers.

---

### Summary

These equations are used to remove the consistent hardware-induced linear bias from phase data. After correction:

- The phase becomes more stable and accurate.
- It's better suited for motion detection, breathing estimation, and other sensing applications.

This preprocessing step is crucial for using CSI phase information effectively.


---

## 4. When to Apply

Apply these preprocessing steps immediately after converting I/Q pairs to phase values, and before any filtering or machine learning.

---

## Summary

Preprocessing of CSI phase data is critical to ensure meaningful analysis. Phase unwrapping eliminates discontinuities, and linear fitting corrects hardware-induced distortions. This results in cleaner, smoother signals better suited for downstream analysis or classification tasks.

