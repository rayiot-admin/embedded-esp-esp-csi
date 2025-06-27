# Applications: Human Presence Detection

Human presence detection is one of the most fundamental and practical applications of CSI-based sensing. By monitoring small variations in the wireless channel, it is possible to determine whether a person is present in a room without using cameras or wearable devices.

---

## 1. Concept

When a human body enters a static wireless environment, it causes changes in signal reflection, diffraction, and absorption. These changes slightly distort the CSI amplitude and phase over time, even when the person is not moving.

This disturbance can be monitored using:
- Variations in CSI amplitude
- Fluctuations in phase (especially if phase has been unwrapped)
- Short-term vs. long-term signal stability

---

## 2. Experimental Setup

- **Environment**: 4 m × 4 m office room, minimal motion, closed doors
- **ESP32 Roles**:
  - TX: Transmitting packets (`active_sta` mode)
  - RX: Receiving and logging CSI (`passive` mode)
- **Distance**: TX-RX gap set to 2.5 meters
- **Test Phases**:
  1. Room empty (baseline data)
  2. Person standing still in center
  3. Person walking slowly across the room

---

## 3. CSI Signal Behavior

### Empty Room:
- CSI amplitude is flat
- Phase variation is minimal and mostly due to noise

### Person Present:
- CSI amplitude shows slight but measurable fluctuation
- Unwrapped phase shows subtle low-frequency oscillation

### Person Walking:
- Rapid phase and amplitude variation
- Higher frequency components appear in signal spectrum

---

## 4. Detection Techniques

- Compute amplitude variance or standard deviation over time
- Apply a low-pass filter to remove high-frequency noise
- Compare current window’s statistics to baseline profile
- Use a binary threshold or machine learning classifier (e.g., SVM)

---

## 5. Sample Output

```plaintext
Time, RSSI, Amplitude Var
2024-06-01 10:03:21.540, -51, 0.027
2024-06-01 10:03:22.540, -51, 0.030
2024-06-01 10:03:23.540, -51, 0.052  <-- person entered room
```
---

## Summary

CSI-based human presence detection leverages subtle shifts in amplitude and phase to identify occupancy in a space. It is a low-cost, privacy-preserving alternative to vision-based systems and can serve as the foundation for smart home or building automation features.

