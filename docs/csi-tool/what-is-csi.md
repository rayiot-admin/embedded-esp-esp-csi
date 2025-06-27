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

The CSI packet format in the ESP32-CSI Tool is based on the `_wifi_csi_cb()` callback and the `wifi_csi_info_t` structure in the ESP-IDF. Each CSI output line begins with `CSI_DATA,` and includes metadata fields followed by raw CSI values.

CSV Header Format:

type,role,mac,rssi,rate,sig_mode,mcs,bandwidth,smoothing,
not_sounding,aggregation,stbc,fec_coding,sgi,noise_floor,
ampdu_cnt,channel,secondary_channel,local_timestamp,ant,
sig_len,rx_state,real_time_set,real_timestamp,len,CSI_DATA

CSI Data Modes:

The output of data->buf depends on compile-time configuration macros:

- `CSI_RAW`: Outputs raw I/Q values in interleaved form: I1 Q1 I2 Q2 ...
- `CSI_AMPLITUDE`: Computes amplitude as sqrt(I² + Q²) per subcarrier
- `CSI_PHASE`: Computes instantaneous phase as atan2(Q, I) per subcarrier

These modes provide flexibility based on your sensing or research needs. Output can be tailored for low-latency logging, high-resolution signal reconstruction, or phase-based inference.

Notes:

- Timestamp is derived from rx_ctrl.timestamp and enhanced by host-side logging
- MAC address and channel info are embedded for filtering and labeling
- Noise floor, SNR, and signal length help evaluate CSI quality

## Sample CSI Output

Example with CSI_RAW:

`CSI_DATA,active_sta,84:f3:eb:12:34:56,-47,26,1,0,1,1,0,0,0,0,1,-95,3,1,0,123456789,0,128,1,1,1685735234576,114,[8 -4 7 1 9 3 10 5 13 6 16 6 17 3 15 1 12 -2 10 -3 ...]`

With CSI_AMPLITUDE:

`CSI_DATA,active_sta,84:f3:eb:12:34:56,-47,26,1,0,1,1,0,0,0,0,1,-95,3,1,0,123456789,0,128,1,1,1685735234576,57,[9 7 9 11 15 17 17 15 12 10 ...]`

With CSI_PHASE:

`CSI_DATA,active_sta,84:f3:eb:12:34:56,-47,26,1,0,1,1,0,0,0,0,1,-95,3,1,0,123456789,0,128,1,1,1685735234576,57,[1 -2 0 1 2 3 2 1 -1 -3 ...]`


## Description of Fields

| Field              | Description                                      |
|--------------------|--------------------------------------------------|
| type               | Always CSI_DATA                                 |
| role               | Project role (e.g., active_sta, passive)        |
| mac                | MAC address of sender                           |
| rssi               | Received Signal Strength Indicator              |
| rate               | Data rate (hex value)                           |
| sig_mode           | Wi-Fi signal mode (0: legacy, 1: HT)            |
| mcs                | Modulation and Coding Scheme                    |
| bandwidth          | 0 = 20 MHz, 1 = 40 MHz                          |
| smoothing          | Whether smoothing is used                       |
| not_sounding       | Packet is not sounding                          |
| aggregation        | Frame was aggregated                            |
| stbc               | Space-Time Block Coding                         |
| fec_coding         | FEC (e.g., LDPC) used                           |
| sgi                | Short Guard Interval used                       |
| noise_floor        | Estimated noise floor                           |
| ampdu_cnt          | Aggregated MPDU count                           |
| channel            | Wi-Fi channel                                   |
| secondary_channel  | Secondary channel indicator                     |
| local_timestamp    | Timestamp from Wi-Fi packet                     |
| ant                | Antenna index                                   |
| sig_len            | Signal length                                   |
| rx_state           | RX state bitmask                                |
| real_time_set      | Indicates if real timestamp is valid            |
| real_timestamp     | Millisecond timestamp from steady clock         |
| len                | Length of CSI buffer in bytes                   |
| CSI_DATA           | Either raw I/Q pairs, amplitudes, or phases     |

## How This Data is Used

Depending on the configuration, CSI data can be analyzed in various ways:

**Raw I/Q Data**: Used for full physical layer modeling and filtering

**Amplitude**: Good for presence detection, gesture recognition, basic signal strength visualization

**Phase**: Used for fine-grained motion sensing, breathing rate estimation, or indoor localization

**Post-processing may include:**

- Phase unwrapping and linear detrending

- Amplitude smoothing

- Subcarrier selection

- FFT or peak counting for periodic signals (e.g., respiration)



## Summary
CSI enables advanced wireless sensing by providing rich signal characteristics per packet. Compared to RSSI, CSI is more informative and allows for fine-grained tracking of environmental changes.

The ESP32-CSI Tool extracts CSI from Wi-Fi frames in real time and outputs it via UART, making it accessible for logging, analysis, and machine learning applications.
