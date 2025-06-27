# Appendices: Dataset Samples

This appendix provides example entries and file structures for raw and preprocessed CSI datasets used in this project. These samples help users understand the formatting, field types, and structure required for analysis, visualization, and machine learning.

---

## 1. Raw CSI Log Sample

Captured using the ESP32 UART stream after enabling CSI in the firmware. This includes all metadata fields and raw I/Q pairs.

```text
CSI_DATA,active_sta,84:f3:eb:12:34:56,-47,26,1,0,1,1,0,0,0,0,1,-95,3,1,0,123456789,0,128,1,1,1685735234576,114,[8 -4 7 1 9 3 10 5 13 6 16 6 17 3 15 1 12 -2 10 -3 ...]
```

---

## 2. Timestamped CSI CSV Sample

Captured using a Python wrapper that prepends UNIX timestamps to each CSI frame during real-time collection.

```text
1685735234576,CSI_DATA,active_sta,84:f3:eb:12:34:56,-47,26,1,0,1,1,0,0,0,0,1,-95,3,1,0,123456789,0,128,1,1,1685735234576,114,[8 -4 7 1 9 3 10 5 13 6 16 6 17 3 15 1 12 -2 10 -3 ...]
```

---

## 3. Structured I/Q CSV Format

Converted using a script to expand I/Q pairs into tabular format with each subcarrier's I and Q values as separate fields.

```text
Timestamp,RSSI,Sub1_I,Sub1_Q,Sub2_I,Sub2_Q,...,Sub52_I,Sub52_Q
1685735234576,-47,8,-4,7,1,9,3,10,5,...,10,-3
```

---

## 4. Amplitude and Phase CSV Format

Derived from I/Q using amplitude = sqrt(I² + Q²) and phase = atan2(Q, I) with unwrapping and linear fitting.

```text
Timestamp,RSSI,Sub1_Amp,Sub1_Phase,Sub2_Amp,Sub2_Phase,...,Sub52_Amp,Sub52_Phase
1685735234576,-47,8.9,2.36,7.2,1.83,...,10.1,-0.54
```

---

## 5. Downloadable Format Suggestions

- Save raw `.csv` logs for reproducibility
- Compress datasets by session: `csi_data_session1_2025-06-25.zip`
- Add metadata in `.txt` format: TX/RX position, room dimensions, environment, label (e.g., LoS/NLoS, breathing, motion)
- Store in `data-files/` directory for easy reference via MkDocs:

Download Sample Output: [sample-1.csv](../data-files/sample-1.csv)

---

## Summary

These dataset formats represent the CSI data processing pipeline—from raw UART logs to structured, timestamped, and filtered datasets. Standardizing these formats ensures consistent preprocessing, interpretability, and reproducibility across experiments.

