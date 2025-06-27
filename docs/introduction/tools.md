# Tools and Hardware Used

The successful execution of CSI-based sensing experiments at Rayiot Solutions Inc.
relies on a carefully selected combination of embedded hardware, software tools,
signal capture utilities, and data analysis frameworks. The embedded team has
standardized the tools and components required to ensure consistent and reproducible
data collection across different experiments and test environments.

This section outlines the specific hardware and software infrastructure used
throughout the project.

## 1. Hardware Components

### ESP32 Development Boards

We primarily use the **ESP32-WROOM-32** module, which offers the following features:

- Dual-core Xtensa® 32-bit LX6 processor
- 2.4 GHz Wi-Fi (802.11 b/g/n)
- UART, SPI, I2C, ADC, and GPIO interfaces
- Onboard antenna or external antenna option (depending on model)

In some test cases, we use variants such as **ESP32-DevKitC** or **ESP32-WROVER**
for flexibility in debugging and USB connectivity.

**Reason for selection:**
- Low cost and widespread availability
- Built-in support for Wi-Fi CSI capture via modified firmware
- USB support for power and serial communication

### Power Supply

- Standard USB 5V power via data cable
- In mobile experiments, **portable power banks** are used to eliminate wall
  interference

### Cabling and Connectors

- USB-A to Micro USB or USB-C depending on board variant
- Jumper wires or breadboards (used in antenna placement or multi-node testing)

### Optional Accessories

- **External Antennas**: Used for improving signal clarity in specific scenarios
- **Stands and Fixtures**: To fix the ESP32 boards at predefined heights and angles
- **Metal-free enclosures**: To isolate reflections in controlled experiments

## 2. Software Tools

### ESP-IDF (Espressif IoT Development Framework)

- Used to build and flash custom CSI-enabled firmware onto ESP32
- Version 4.2 or higher is required for CSI support

**Key Commands Used:**
- `idf.py build`
- `idf.py flash`
- `idf.py monitor`

### Python 3.8+

Used extensively for:
- Serial data parsing
- Phase unwrapping and linear fitting
- Saving data to CSV
- Plotting amplitude and phase (optional)

**Essential Libraries:**
- `pyserial`: Reading data from UART
- `numpy`, `scipy`: Signal processing
- `matplotlib`: Plotting
- `pandas`: Data manipulation

### Serial Monitors

For quick inspection and debugging of raw UART output:
- `minicom` (Linux)
- `PuTTY` (Windows)
- `screen` or `idf.py monitor` (cross-platform)

### Git and Version Control

All firmware builds, test configurations, and scripts are version-controlled using Git,
ensuring reproducibility and traceability.

- GitHub is used for codebase storage
- Internal wiki and Git issues track build results, bugs, and release notes

## 3. Data Analysis & Visualization

Although preprocessing is minimal on the embedded side, initial exploratory analysis
is performed using the following tools:

- **Jupyter Notebooks** for interactive data exploration
- **CSV file viewers** for raw inspection
- **Custom scripts** for plotting unwrapped phase vs time
- **Wireshark** (for PCAP-based analysis when applicable)

## 4. Documentation and Collaboration Tools

To document and share findings internally and with adjacent teams:
- **MKDocs** with Material Theme for structured documentation
- **Google Drive / Shared Folders** for image and dataset storage
- **Notion / Internal wiki** for notes and experiment tracking
- **Slack / Microsoft Teams** for updates and coordination across embedded, DSP,
  and AI teams

## Summary

The combination of reliable ESP32 hardware, open-source development tools, and
internally developed data handling scripts enables the embedded team at Rayiot
Solutions Inc. to operate a consistent CSI collection pipeline. This tooling
infrastructure forms the foundation for downstream tasks like signal processing,
model training, and system validation for breathing rate estimation and related
use cases.

