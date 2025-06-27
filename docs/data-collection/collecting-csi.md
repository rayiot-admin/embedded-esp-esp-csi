# Collecting CSI Data

Once the ESP32 is configured and flashed with the CSI-enabled firmware, you can begin collecting CSI data using either serial output or onboard SD card logging (if supported by the board).

This section outlines methods for capturing CSI data in real time.

---

## 1. Using `idf.py monitor`

This is the simplest way to view and capture CSI output from the ESP32.

### Start the monitor:
```bash
idf.py monitor
```

This will show real-time serial output, including CSI data if your configuration is correct.

To exit the monitor, press:
```
Ctrl + ]
```

---

## 2. Logging CSI Data to a File

You can redirect CSI output to a file using standard shell tools.

### On Linux/macOS:
```bash
idf.py monitor | grep "CSI_DATA" > csi_output.csv
```

### On Windows (PowerShell):
```powershell
idf.py monitor | findstr "CSI_DATA" > csi_output.csv
```

This will capture only lines containing CSI readings and store them in a CSV file.

---

## 3. Adding Timestamps (Optional)

The ESP32 does not attach timestamps to CSI packets. To correlate CSI with time, use the `serial_append_time.py` script.

### Example:
```bash
idf.py monitor | python ../python_utils/serial_append_time.py > csi_output_timed.csv
```

Each CSI line will be prepended with a UNIX timestamp.

---
## 4. Final way of collecting CSI data

The ESP32 does not attach timestamps to CSI packets. Use custom scripts to collect the csi data along with timestamps.

### Example:
```bash
idf.py -p /dev/ttyUSB0 monitor | python ../python_utils/phase_wrapped_filter_linear_fitting_v0.1.py > sample-1.csv
```

Each CSI line will be prepended with a UNIX timestamp.

Download Sample Output: [sample-1.csv](../data-files/sample-1.csv)

---


## 5. SD Card Logging (Optional)

If using a board like the TTGO T8 with microSD support:
- CSI data can be stored directly to a text or CSV file on the SD card
- This is useful for mobile or battery-powered experiments

Refer to `active_sta/sd_logger.c` or related examples in the ESP32-CSI Tool repository.

---

## 6. Monitoring Stability

- Ensure baud rate in your terminal matches what was configured in `menuconfig`
- Minimize system load on your host to avoid dropped packets
- If CSI stops or stutters, restart the monitor and reset the ESP32

---

## Summary

CSI data can be collected either over serial (using `idf.py monitor`) or by logging to a file. Timestamps can be added for analysis using Python scripts. For field tests, boards with SD card support can store CSI data locally.

