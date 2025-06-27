# Setting Up the Environment

This section provides a complete walkthrough to set up the development environment required to build, flash, and use the ESP32-CSI Tool on your local machine.

It includes ESP-IDF installation, firmware cloning, configuration, and setup for serial monitoring and CSI data capture.

---

## 1. Prerequisites

Before starting, ensure you have:

- A stable internet connection
- An ESP32 DevKit board
- Python 3.8 or higher installed
- Basic familiarity with command-line interface

---

## 2. Install ESP-IDF (v4.3 or higher)

ESP-IDF (Espressif IoT Development Framework) is the official SDK for ESP32 development.

### Installation Steps (Linux/macOS):

```bash
git clone --recursive https://github.com/espressif/esp-idf.git
cd esp-idf
git checkout v4.3
./install.sh
. ./export.sh
```

To avoid re-running `. ./export.sh` every time, add it to your shell config:

```bash
echo ". $HOME/esp-idf/export.sh" >> ~/.bashrc
```

> Windows users should use the [ESP-IDF Tools Installer](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/windows-setup.html)

---

## 3. Clone the ESP32-CSI Tool Repository

Clone the CSI-enabled firmware project:

```bash
git clone https://github.com/StevenMHernandez/ESP32-CSI-Tool.git
cd ESP32-CSI-Tool
```

---

## 4. Choose the Operating Mode

Depending on your use case, enter one of the following directories:

```bash
cd active_sta    # Active Station mode (transmitting packets)
cd active_ap     # Active Access Point mode (collecting packets)
cd passive       # Passive CSI capture (only listen)
```

> `passive` mode is often used for RX-only logging setups.

---

## 5. Configure the Firmware (`idf.py menuconfig`)

Run the configuration menu:

```bash
idf.py menuconfig
```

### Important settings:
- **Component Config → Wi-Fi**: Enable Wi-Fi CSI (Channel State Information)
- **Serial flasher config**: Set baud rate to at least 921600 or 1152000
- **FreeRTOS**: Set tick rate to **1000 Hz** (under Component Config → FreeRTOS)
- **Console output baud rate**: Must match the flashing baud rate

Once configured, save and exit.

---

## 6. Build and Flash the Firmware

Compile and upload the firmware to the ESP32:

```bash
idf.py build
idf.py flash
```

---

## 7. Monitor Serial Output

To monitor live CSI data:

```bash
idf.py monitor
```

If you only want CSI lines logged:

```bash
idf.py monitor | grep "CSI_DATA" > csi_output.csv
```

To add timestamps:

```bash
idf.py monitor | python ../python_utils/serial_append_time.py > csi_output_timed.csv
```

> Make sure your serial port baud matches what was set in `menuconfig`.

---

## 8. Python Environment for CSI Scripts (Optional)

If you plan to parse or visualize CSI data:

```bash
python3 -m venv venv
source venv/bin/activate
pip install pyserial numpy matplotlib pandas
```

> Optional scripts are located in `python_utils/` inside the ESP32-CSI repo.

---

## 9. Useful Python Scripts

Inside `ESP32-CSI-Tool/python_utils/`:

| Script                        | Purpose                                 |
|------------------------------|-----------------------------------------|
| `serial_plot_csi_live.py`    | Live plotting of CSI amplitude          |
| `serial_append_time.py`      | Add timestamps to each CSI log line     |
| `serial_to_dataframe.py`     | Convert CSI logs to structured pandas df|

---

## 10. Troubleshooting Tips

- **Flashing fails at 0%**: Check USB cable (must be data cable) or switch port
- **No CSI output**: Re-check `menuconfig` settings for CSI enable
- **Garbled serial output**: Baud mismatch — align `menuconfig` and terminal
- **Monitor hangs**: Reduce baud or check power stability

---

## Summary

After following this guide, you should have:

- ESP-IDF installed and exported
- ESP32-CSI Tool firmware cloned and configured
- ESP32 board flashed and logging CSI data
- (Optional) Python environment set up for live visualization

You’re now ready to move on to data collection and analysis.

