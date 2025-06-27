# Working of CSI Tool

This section explains the underlying principles and technical background of how the ESP32-CSI Tool functions. It introduces Channel State Information (CSI), compares it with traditional metrics like RSSI, and outlines how CSI is captured using the ESP32 chip.

---

## What is CSI?

Channel State Information (CSI) refers to detailed information about the wireless channel through which a signal travels from a transmitter to a receiver. CSI captures how each subcarrier in an OFDM-based Wi-Fi signal is affected by the surrounding environment.

Unlike RSSI (Received Signal Strength Indicator), which gives a single value for signal strength, CSI provides:
- Amplitude and phase for each subcarrier
- Per-packet frequency-domain data
- Real-time responsiveness to small environmental changes

### Why CSI Matters
CSI enables fine-grained sensing, making it useful for applications like:
- Human presence detection
- Breathing rate estimation
- Gesture recognition
- Indoor localization

---

## CSI vs. RSSI

| Feature         | RSSI                        | CSI                                     |
|----------------|-----------------------------|------------------------------------------|
| Data Granularity | Single value                | Per subcarrier (e.g., 52 for 20 MHz)     |
| Captures Phase  | No                          | Yes                                      |
| Use Cases       | Connectivity estimation     | Sensing, tracking, motion detection      |
| Sensitivity     | Low                         | High (reacts to small physical changes)  |

---

## How CSI is Captured by ESP32

The ESP32-CSI Tool works by enabling receiving(AP) mode on the ESP32’s Wi-Fi chipset and extracting CSI data from received packets. The firmware modifies low-level Wi-Fi driver functions to expose CSI data via UART.

### Process Overview
1. **ESP32 receives Wi-Fi packets in receiving(AP) mode**
2. **CSI extraction is triggered per frame**
3. **Data is formatted into UART strings with MAC info, RSSI, amplitude, and phase**
4. **UART stream is logged on the host machine**

---

## CSI Packet Structure
Each CSI message from the ESP32 includes:
- Timestamp
- MAC address
- RSSI
- Channel and rate info
- CSI payload (complex values for each subcarrier)

The data is typically parsed and saved in CSV format or visualized live using Python scripts.

---

## Sample CSI Output

```
CSI_DATA, MAC=84:f3:eb:12:34:56, CH=1, RSSI=-47, RATE=0x1a, SIG_MODE=1, MCS=0, CWB=1, SNR=42, Noise Floor=-95
CSI_LEN=114, CHAN=1, Secondary Channel=0, RSSI=-47, First Word=242
Timestamp: 1685735234576
CSI: 
8,-4 7,1 9,3 10,5 13,6 16,6 17,3 15,1 12,-2 10,-3 ...
```

### Description of Fields

| Field              | Description                                               |
|--------------------|-----------------------------------------------------------|
| MAC                | MAC address of the transmitter                            |
| CH                 | Wi-Fi channel number                                      |
| RSSI               | Received Signal Strength Indicator (dBm)                  |
| RATE, MCS          | Transmission rate and Modulation and Coding Scheme        |
| SIG_MODE           | Signal mode (1 = HT, 0 = legacy)                          |
| CWB                | Channel Bandwidth (1 = 40 MHz, 0 = 20 MHz)                |
| SNR, Noise Floor   | Signal-to-Noise Ratio and reference baseline              |
| CSI_LEN            | Number of subcarriers captured                            |
| Timestamp          | Time of packet capture (ms)                               |
| CSI                | List of complex pairs (I, Q) per subcarrier               |

### How This Data is Used

- Amplitude: `sqrt(I² + Q²)` for each subcarrier
- Phase: `atan2(Q, I)` after unwrapping and filtering
- Can be plotted over time to detect motion, respiration, etc.


## Summary
CSI enables advanced wireless sensing by providing rich signal characteristics per packet. Compared to RSSI, CSI is more informative and allows for fine-grained tracking of environmental changes.

The ESP32-CSI Tool extracts CSI from Wi-Fi frames in real time and outputs it via UART, making it accessible for logging, analysis, and machine learning applications.

