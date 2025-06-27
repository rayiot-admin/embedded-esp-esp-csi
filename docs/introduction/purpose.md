# Purpose of This Documentation

At Rayiot Solutions Inc., our embedded systems team has undertaken a series of
initiatives centered around using Wi-Fi-based sensing technologies to enable
non-invasive human monitoring, gesture recognition, and indoor localization.
A key enabler of these capabilities is the ESP32-CSI Tool — an open-source utility
that allows us to extract Channel State Information (CSI) from ESP32-based devices.

This documentation has been created to formalize our internal work, streamline
collaboration across teams, and serve as a long-term technical reference for
projects involving CSI-based sensing.

## Objectives of This Documentation

### 1. To Establish Technical Clarity and Knowledge Sharing

Our embedded team has invested time in understanding the operational nuances of the
ESP32-CSI Tool — from environment setup, firmware modification, and UART-based data
capture, to real-time processing and visualization of CSI data. This documentation
is intended to consolidate that knowledge and make it accessible for onboarding new
engineers, collaborating with AI and data teams, and supporting system-level
integration with our cloud and mobile platforms.

### 2. To Standardize Setup and Experimentation

Different engineers working on different modules of the Rayiot sensing pipeline
need a consistent and reliable CSI data collection and visualization setup. This
documentation provides a reproducible process — including hardware wiring,
flashing instructions, configuration steps, and supporting scripts — to ensure
alignment across devices, locations, and development teams.

### 3. To Support Application Prototyping and Feature Evaluation

Our use of CSI is directly aligned with our product goals in smart health,
behavioral sensing, and contactless monitoring. By documenting various experiments
such as presence detection, fall detection, and motion classification, we aim to
accelerate the prototyping of features that can be incorporated into our Raybaby
platform or future product lines.

### 4. To Identify Limitations and Engineering Risks Early

The CSI pipeline introduces several engineering challenges: real-time data
stability, signal quality under environmental noise, variability between ESP32
modules, and data synchronization across multiple sensing nodes. This documentation
serves as a log of technical blockers, firmware anomalies, and configuration
constraints observed during our evaluation, along with any workarounds or
recommendations.

### 5. To Align Embedded, AI, and DSP Teams

Our CSI-based sensing work does not end at raw data collection. The captured data
is analyzed, filtered, and used to train AI models — which in turn must integrate
with mobile or cloud platforms. By making this documentation the single source of
truth, we ensure all teams — embedded, AI, cloud, and app development — have
access to the assumptions, parameters, and data interpretation practices used at
each stage.

## Scope of the Documentation

This documentation includes:

- Overview of CSI and its relevance to Rayiot use cases
- Setup instructions for the ESP32-CSI Tool and supporting environment
- Standard operating procedures for data collection
- Data formats and example outputs
- Common signal patterns for test scenarios (e.g., human movement, gestures)
- Challenges faced during firmware development and hardware calibration
- Sample visualizations and interpretation notes
- Guidelines for handing over data to the AI team

## Audience

The primary audience for this documentation includes:

- Embedded developers working on firmware and device-side CSI capture
- Data scientists building models from CSI logs
- QA teams validating sensing accuracy across devices and scenarios
- Product engineers evaluating the feasibility of CSI-based features
- Interns or new hires onboarding into the Rayiot sensing pipeline

## Summary

This documentation is part of Rayiot Solutions Inc.'s internal knowledge base for
Wi-Fi sensing and CSI-enabled embedded development. It supports current and future
projects by ensuring that engineering efforts are well-documented, reproducible,
and aligned across teams. It reflects our broader goal of developing robust,
scalable, and intelligent sensing systems using low-cost hardware like the ESP32.

