# Software Requirements

This section outlines the software tools, environments, and dependencies required to work with the ESP32-CSI Tool. These tools enable firmware development, serial data capture, preprocessing, and initial visualization.

---

## 1. Operating System

The ESP32-CSI Tool can run on all major desktop operating systems:

| OS             | Status     | Recommended Version         |
|----------------|------------|-----------------------------|
| Windows        | ✅ Supported | Windows 10 or 11            |
| Linux          | ✅ Preferred | Ubuntu 20.04+ or Debian 11  |
| macOS          | ✅ Supported | macOS 12+                   |

For development purposes, **Linux (Ubuntu)** is preferred due to seamless ESP-IDF compatibility and serial tool support.

---

## 2. ESP-IDF (Espressif IoT Development Framework)

ESP-IDF is the official development environment for ESP32. It is required for building and flashing the custom CSI-enabled firmware.

> 🔗 For detailed setup instructions, see [Setting Up the Environment](../getting-started/environment.md).

Minimum required version: `v4.3`

---

## 3. Python Environment

You will need **Python 3.8 or higher** installed on your machine to run CSI logging and visualization scripts.

Recommended libraries:
- `pyserial`: For reading CSI data via UART
- `numpy`, `pandas`: For processing data arrays and tabular data
- `matplotlib`: For plotting CSI amplitude and phase

Install them using:

```bash
pip install pyserial numpy matplotlib pandas
```

> 🔗 For virtual environment setup and best practices, refer to [Setting Up the Environment](../getting-started/environment.md).

---

## 4. Serial Monitoring Tools

You can use any of the following tools to monitor CSI data over serial:

| Tool         | Platform       | Notes                             |
|--------------|----------------|------------------------------------|
| `idf.py monitor` | All (in ESP-IDF) | Built-in monitor, recommended     |
| `minicom`    | Linux           | Terminal-based, lightweight        |
| `PuTTY`      | Windows         | GUI-based, widely used             |
| `screen`     | macOS/Linux     | Terminal-based serial monitor      |

---

## 5. Git & Version Control

Git is essential for cloning the ESP32-CSI Tool repository and versioning your changes.

Basic commands:

```bash
git clone https://github.com/StevenMHernandez/ESP32-CSI-Tool.git
cd ESP32-CSI-Tool
```

> 🔗 Refer to [Setting Up the Environment](../getting-started/environment.md) for project mode selection and build configuration.

---

## References

Much of the ESP-IDF setup and configuration guidance in this document is based on:

- Steven M. Hernandez, [ESP32-CSI Tool Documentation](https://stevenmhernandez.github.io/ESP32-CSI-Tool/), GitHub Pages, MIT Licensed.

> Portions of this documentation are adapted from the ESP32-CSI Tool by Steven M. Hernandez, which is licensed under the MIT License.

---

## Summary

To run and extend the ESP32-CSI Tool successfully, the following software stack is required:

- A compatible operating system (Ubuntu recommended)
- ESP-IDF v4.3+ (see environment setup)
- Python 3.8+ with required libraries
- Serial monitor tools (CLI or GUI)
- Git for cloning and version control

Once these tools are installed, follow [Setting Up the Environment](../getting-started/environment.md) to flash the firmware and begin CSI data collection.

