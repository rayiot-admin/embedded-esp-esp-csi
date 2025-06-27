# Optimization & Improvements

During the development and experimentation with CSI-based sensing, several optimization strategies were introduced to enhance data quality, signal consistency, and processing efficiency. These improvements targeted both firmware behavior and host-side processing.

---

## 1. UART and Serial Logging Optimization

### Problem
- Dropped packets and serial corruption at lower baud rates

### Solution
- Increased UART baud rate to 921600 or 1152000
- Filtered CSI logs to remove non-essential output
- Used timestamped logging to structure CSI entries on host side

---

## 2. Phase Preprocessing Enhancements

### Problem
- Wrapped and noisy phase values reduced signal reliability

### Solution
- Developed custom unwrapping and linear detrending logic (inspired by phase_wrapped_filter_linear_fitting.py)
- Preprocessing reduced hardware bias and restored continuity across frames
- Applied normalization to amplitude and detrended phase before ML pipelines

---

## 3. Subcarrier Selection Strategy

### Problem
- Inconsistent signal quality across subcarriers

### Solution
- Performed statistical variance analysis to identify stable subcarriers
- Excluded edge subcarriers and those with high-frequency noise
- Focused on ~20–30 central subcarriers for respiration and presence tasks

---


## 4. Tooling and Data Handling

### Improvements
- Created custom CSV parsers to clean malformed CSI logs
- Used Pandas and Numpy for rapid batch preprocessing
- Enabled live plots using Matplotlib for in-session tuning

---

## 5. Experiment Repeatability

### Steps Taken
- Documented environment conditions, TX/RX positions, and signal settings
- Used fixed mounting tripods and printed floor markers
- Replayed data logs for consistent benchmarking

---

## Summary

These optimization techniques helped make CSI sensing more robust, consistent, and portable across different environments. From UART tuning to smarter subcarrier filtering and dynamic thresholds, each step contributed to cleaner signals and improved real-world usability.

