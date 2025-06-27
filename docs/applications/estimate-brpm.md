# Applications: Estimation of Breathing Rate (BrPM)

Breathing rate estimation using CSI is a powerful, non-contact method for monitoring human respiration in real-time. The small, periodic movements of the chest during inhalation and exhalation induce micro-level changes in CSI phase and amplitude.

---

## 1. Principle

Breathing causes periodic variations in signal reflections due to the slight movement of the chest wall. These changes primarily manifest as:

- Low-frequency oscillations in **unwrapped phase**
- Subtle amplitude shifts in static environments

The frequency of these oscillations can be mapped directly to **breaths per minute (BrPM)**.

---

## 2. Experimental Setup

- **Environment**: Quiet, temperature-stable room
- **Person State**: Sitting still, breathing normally
- **Distance**: TX-RX gap ~1.5 m
- **ESP32 Modes**:
  - TX in `active_sta`
  - RX in `passive` or `active_ap`
- **Data Type**: Unwrapped and detrended phase across selected subcarriers

---

## 3. Signal Processing Steps

1. Capture and unwrap phase using `atan2(Q, I)` and `numpy.unwrap`
2. Detrend phase using linear fitting (to remove DC bias)
3. Apply a band-pass filter (~0.1–0.5 Hz)
4. Perform Fast Fourier Transform (FFT) or peak counting over sliding windows
5. Estimate BrPM using:

```python
brpm = peak_count * (60 / window_duration_in_seconds)
```

---

## 4. Sample Visualization

### Example Output:

```plaintext
Time Window Start: 2024-06-01 10:15:00
Detected Peaks: 6
Window Duration: 30 seconds
Estimated BrPM: 12
```
---

## 5. Challenges and Notes

- Ensure minimal movement for clean respiration signal
- Signal may vary significantly by subcarrier; select the most stable one
- Ambient air flow, talking, or shifting posture can distort the signal

---

## Summary

By processing low-frequency oscillations in CSI phase data, we can estimate breathing rate (BrPM) reliably in static conditions. This technique is highly applicable in health monitoring systems for infants, patients, or elderly individuals without requiring contact-based sensors.

