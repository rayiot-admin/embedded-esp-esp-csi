# Configuration of ESP32

This section describes how to configure your ESP32 boards to collect Channel State Information (CSI) data using the ESP32-CSI Tool. Configuration is done through the ESP-IDF’s `menuconfig` interface prior to building and flashing the firmware.

---

## 1. Launch menuconfig

Navigate to your ESP32-CSI Tool project directory and run:

```bash
idf.py menuconfig
```

This opens a terminal-based configuration menu.

---

## 2. Key Configuration Settings

Below are the essential settings to be modified:

### A. Enable CSI
- Navigate to: `Component config → Wi-Fi`
- Enable: `Enable Wi-Fi CSI (Channel State Information)`

### B. Set Wi-Fi Channel
- Choose a **fixed Wi-Fi channel** (e.g., channel 1, 6, or 11)
- Ensure your router or transmitting device is using the same channel

### C. Set Serial Flasher Baud Rate
- Navigate to: `Serial flasher config`
- Set: `921600` or higher (`1152000` recommended for better throughput)

### D. Console Baud Rate
- Set console output to match the flash baud rate
- Typically: `921600` or `1152000`

### E. FreeRTOS Tick Rate
- Navigate to: `Component config → FreeRTOS`
- Set: `Tick rate (Hz)` to `1000`

### F. MAC Filter (Optional)
- Some modes allow filtering CSI by source MAC address
- Can be used to isolate CSI from a specific transmitter

---

## 3. Save and Exit

After configuration:
- Press `S` to save
- Press `Q` to exit the menu

Configuration settings are stored in the `sdkconfig` file in your project directory.

---

## 4. Rebuild the Firmware

After saving your configuration, rebuild and flash the firmware:

```bash
idf.py build
idf.py flash
```

Make sure to monitor for successful upload and verify CSI output after boot.

---

## Summary

Proper configuration of your ESP32 is essential for reliable CSI data collection. Ensure CSI is enabled, Wi-Fi channel is fixed, baud rates are set high, and tick rate is optimized. These settings significantly impact data consistency and integrity.

