# Challenges Faced

While implementing CSI-based sensing using the ESP32-CSI Tool, several technical and practical challenges were encountered during experimentation and deployment. These challenges span hardware limitations, data consistency, and environmental factors.

---

## 1. Hardware Limitations

### A. UART Bandwidth
- High CSI sampling rates produce large amounts of data
- The ESP32 UART struggles at lower baud rates
- Required use of high-speed rates (≥921600), which are not always stable across devices

### B. Memory Constraints
- Limited RAM on ESP32 makes buffering long CSI frames difficult
- Cannot support continuous high-rate CSI logging without pauses or drops

### C. No Native Timestamp
- CSI frames from ESP32 do not include native timestamps
- Timestamping must be added on host side, introducing alignment errors

---

## 2. Environmental Variability

### A. Multipath Instability
- Reflections from furniture, people, or walls cause signal fluctuations
- Even minor environmental changes can impact phase data stability

### B. Signal Interference
- Nearby Wi-Fi networks, mobile hotspots, or Bluetooth devices interfere with CSI capture
- Requires careful channel selection and isolation

---

## 3. Data Processing Issues

### A. Phase Wrapping
- Phase values are wrapped within \( \pm\pi \), leading to discontinuities
- Requires robust unwrapping and detrending logic

### B. Subcarrier Noise
- Not all subcarriers are equally reliable
- Selecting the most stable subcarriers requires manual testing or heuristics

### C. Synchronization Gaps
- Multi-device setups (for localization or tracking) require synchronized clocks
- ESP32 lacks precise time synchronization unless external methods are added

---

## 4. Software and Tooling Challenges

- ESP-IDF version compatibility with CSI firmware
- Limited support for real-time filtering and visualization in the default tool
- Logging format varies slightly across modes (active_ap, active_sta, passive)

---

## 5. Overcoming the Challenges

### UART Bandwidth
- Used higher baud rate (921600 or 1152000) for UART to prevent data loss.
- Ensured serial monitor tools (like `idf.py monitor`) are configured to handle this rate.

### Memory Constraints
- Kept CSI processing minimal on-device.
- Offloaded preprocessing to host side to avoid RAM bottlenecks.

### Timestamps
- Added timestamps using a Python wrapper script on the host system (`serial_append_time.py`).
- Customized to prepend high-resolution timestamps.

### Phase Wrapping and Bias
- Applied phase unwrapping and linear fitting using in-house Python scripts.
- Used logic inspired by:

```python
# Phase unwrapping
for i in range(len(phases) - 1):
    phases[i + 1] = unwrap_phase(phases[i], phases[i + 1])

# Linear fitting
alpha_1 = (phases[F] - phases[0]) / (2 * pi * F)
alpha_0 = sum(phases) / F
for i in range(len(phases)):
    phases[i] -= (alpha_1 * i + alpha_0)
```

### Subcarrier Noise
- Selected stable subcarriers empirically through visual inspection.
- Ignored edge subcarriers which tend to be noisier.

### Interference and Multipath
- Performed experiments during idle network hours.
- Used isolated channels and quiet test environments.

### Synchronization Gaps
- Single-device experiments avoided clock sync issues.
- For multi-device setups, network time sync or NTP can be integrated.

---

## Summary

CSI-based sensing offers significant potential, but also presents multiple technical and operational challenges. Addressing them requires careful firmware configuration, robust signal processing, and consistent test environments. Acknowledging and documenting these challenges is key to improving system reliability and performance.

