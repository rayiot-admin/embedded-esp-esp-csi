# Appendices: Code Samples

This appendix provides a collection of code snippets used throughout the project for CSI collection, processing, and visualization.

---

## 1. Serial Logging with Timestamps

Add real-time timestamps to CSI data from ESP32 UART stream:

```python
import time
import serial

ser = serial.Serial('/dev/ttyUSB0', 921600)

with open('csi_output_timed.csv', 'w') as out:
    while True:
        line = ser.readline().decode(errors='ignore').strip()
        if 'CSI_DATA' in line:
            timestamp = int(time.time() * 1000)
            out.write(f"{timestamp}, {line}\n")
```

---

## 2. Amplitude and Phase Calculation

Convert raw I/Q samples to amplitude and phase:

```python
import numpy as np

# Example raw CSI line (interleaved I/Q pairs)
csi_raw = [8, -4, 7, 1, 9, 3, 10, 5]  # I1,Q1, I2,Q2, ...
i_vals = csi_raw[0::2]
q_vals = csi_raw[1::2]

amplitude = np.sqrt(np.square(i_vals) + np.square(q_vals))
phase = np.arctan2(q_vals, i_vals)
```

---

## 3. Phase Unwrapping and Linear Detrending

```python
from scipy.stats import linregress

# Unwrap phase
delta = np.diff(phase)
delta[delta > np.pi] -= 2 * np.pi
delta[delta < -np.pi] += 2 * np.pi
unwrapped = np.cumsum(np.insert(delta, 0, phase[0]))

# Remove linear bias
x = np.arange(len(unwrapped))
slope, intercept, _, _, _ = linregress(x, unwrapped)
detrended = unwrapped - (slope * x + intercept)
```

---

## 4. Plotting Amplitude

```python
import matplotlib.pyplot as plt

plt.plot(amplitude)
plt.title("CSI Amplitude")
plt.xlabel("Subcarrier Index")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
```

---

## 5. Reference – CSI Preprocessing Script

The following operations are implemented in the custom CSI preprocessing script developed by RayIoT Solutions:

- Captures CSI data from standard input
- Extracts I/Q values from CSI frames
- Converts I/Q into amplitude and phase
- Applies phase unwrapping to remove discontinuities
- Applies linear detrending to eliminate hardware bias
- Adds timestamp in milliseconds to each CSI log entry

### File: `phase_wrapped_filter_linear_fitting_v0.1_original.py`

```python
# Extract real and imaginary parts
for i in range(len(csi_raw)):
    if i % 2 == 0:
        imaginary.append(csi_raw[i])
    else:
        real.append(csi_raw[i])

# Calculate amplitude and phase
for i in range(frame_len):
    amplitudes.append(sqrt(imaginary[i] ** 2 + real[i] ** 2))
    phases.append(atan2(imaginary[i], real[i]))

# Phase unwrapping loop
for i in range(frame_len - 1):
    phases[i + 1] = unwrap_phase(phases[i], phases[i + 1])

# Apply linear fit to sanitize unwrapped phase
phase_filter_linear_fit(phases)

# Append timestamp and print
print(f"{formatted_time},{rssi}")
```

---

## Summary

These code samples represent core utilities used for working with CSI data from ESP32. Each can be adapted and extended for custom use cases like real-time analysis or batch training pipelines.

