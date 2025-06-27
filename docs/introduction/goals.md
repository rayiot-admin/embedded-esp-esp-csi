# Goals of the Project Using CSI

At Rayiot Solutions Inc., one of our key initiatives is to develop a contactless,
non-invasive system capable of estimating human breathing rates using Wi-Fi signal
variations. Channel State Information (CSI), extracted from the ESP32 Wi-Fi chipset,
serves as the foundational signal input for this system.

The primary focus of the embedded systems team is to establish a robust CSI
acquisition pipeline, preprocess the data in a meaningful format, and provide a
clean, high-resolution signal to downstream DSP and AI modules. This section
outlines the goals and deliverables of the embedded team in this context.

## 1. To Collect High-Quality CSI Data

The first and foremost goal is to set up an environment that consistently produces
clean and stable CSI data under various conditions. This involves:

- Selecting and configuring suitable ESP32 hardware modules
- Flashing the CSI-enabled firmware with optimized UART and sampling parameters
- Ensuring proper placement and orientation of TX and RX devices
- Isolating environmental noise as much as possible
- Logging CSI packets with minimal packet loss or corruption

The collected CSI should cover diverse scenarios, including static, single-subject
breathing conditions in quiet environments.

## 2. To Extract and Format Key Signal Components

After CSI data is captured over UART, it is processed to extract the following
critical signal components:

- **Timestamps:** To correlate CSI samples and track changes over time
- **Amplitude values:** Calculated from the real and imaginary components per
  subcarrier, used to observe periodic variations from breathing
- **Phase values:** Cleaned and unwrapped to analyze finer signal characteristics
- **RSSI (Received Signal Strength Indicator):** Used as an auxiliary reference
  metric for signal quality

The processed output is structured in a consistent format (CSV/JSON) with each
entry timestamped and annotated for analysis.

## 3. To Implement Lightweight Preprocessing

The embedded team currently focuses on essential phase preprocessing techniques to
ensure that the raw CSI data is usable for downstream signal analysis.

The following operations are implemented:

- **Phase sanitization:** Filtering out invalid or unstable phase values caused by
  noise or hardware-induced anomalies.
- **Phase unwrapping:** Correcting discontinuities in the raw phase values that
  result from wrapping around the [-π, π] range.
- **Linear fitting:** Applying linear regression to the phase across subcarriers to
  remove linear trends (caused by hardware delays), isolating meaningful variations.

These preprocessing steps are lightweight, non-computationally intensive, and
performed in real-time or near-real-time as part of the UART CSI data parsing
pipeline.

At this stage, no amplitude filtering, outlier removal, or subcarrier averaging
is performed. The focus remains on producing clean, unwrapped, and detrended phase
data to support accurate frequency-domain analysis by the DSP team.

## 4. To Package and Hand-Off Data for DSP and AI Pipelines

Once data is collected and processed, it is passed to the downstream teams:

- **DSP Team:** Applies frequency-domain analysis, bandpass filtering, and signal
  isolation to extract the breathing envelope
- **AI Team:** Uses preprocessed data to train and evaluate models that estimate
  breathing rate under various conditions

To support this, the embedded team ensures:

- The file formats and directory structure are standardized
- Metadata (e.g., sampling rate, test ID, subject info) is included
- Data integrity is verified and logged before upload or transfer

## 5. To Iterate and Improve Data Fidelity

As part of an iterative process, the team constantly reviews the following:

- Packet capture success rate
- Signal-to-noise ratio of the extracted features
- Alignment of signal features with expected physiological behavior

Feedback from DSP and AI teams is looped back to improve ESP32 configuration,
hardware layout, and firmware tuning.

## Summary

The goal of this CSI-based project at Rayiot Solutions Inc. is not only to collect
raw wireless data but to turn it into a reliable, signal-rich source that can power
intelligent sensing applications. The embedded team plays a critical role in laying
the groundwork by building a stable, configurable, and reproducible CSI pipeline
optimized for breathing rate estimation and future expansion into other health and
behavioral sensing tasks.

