# Data Analysis: Raw CSI Format

This section explains the structure and content of raw Channel State Information (CSI) output from the ESP32-CSI Tool. Understanding this format is essential for parsing, filtering, and visualizing the data.

---

## 1. Structure of a CSI Data Packet

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

### Example Raw Output:
```
`CSI_DATA,active_sta,84:f3:eb:12:34:56,-47,26,1,0,1,1,0,0,0,0,1,-95,3,1,0,123456789,0,128,1,1,1685735234576,114,[8 -4 7 1 9 3 10 5 13 6 16 6 17 3 15 1 12 -2 10 -3 ...]`
```

---

## 2. Metadata Field Descriptions

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

---

## 3. CSI Payload Format

The CSI line consists of a flat sequence of **complex-valued pairs** representing the I (in-phase) and Q (quadrature) components for each subcarrier:

```
CSI: I1,Q1 I2,Q2 I3,Q3 ... In,Qn
```

These values represent the channel’s response across frequency and are later used to derive amplitude and phase.

### Amplitude:
```
Amplitude = sqrt(I^2 + Q^2)
```

### Phase:
```
Phase = atan2(Q, I)
```

Phase data often requires additional unwrapping and linear fitting to remove hardware bias and noise.

---

## 4. Notes on Parsing and Resolution

- A 20 MHz channel usually provides 52 subcarriers (26 left, 26 right of center)
- ESP32 CSI output may vary depending on mode (active/passive), firmware version, and ESP-IDF
- Some fields like `SNR` or `Noise Floor` may be placeholder values unless explicitly enabled in firmware

---

## 5. Typical Range of Values

### I/Q Samples
- I/Q pairs usually range from -128 to +127
- Represent 8-bit signed integer values depending on ESP32 ADC resolution and format

### Amplitude Values
- Generally fall between 0 to ~180 depending on movement, multipath, and distance

### Phase Values
- Range from -π to +π (in radians) before unwrapping
- After processing, can be used to detect Doppler shifts or relative motion

---


## Summary

Raw CSI format includes a metadata header followed by a flat list of I/Q pairs. These complex values are the foundation for computing signal amplitude and phase per subcarrier and are essential for building CSI-based sensing and inference systems.

