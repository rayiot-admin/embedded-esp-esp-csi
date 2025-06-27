# How CSI Data is Collected

The ESP32-CSI Tool collects CSI data by placing the ESP32 Wi-Fi chipset into stationary mode and modifying its internal firmware to expose CSI measurements from received Wi-Fi frames. This process is based on features available in the Espressif SDK but requires specific driver changes that are implemented in the ESP32-CSI Tool firmware.

## Modes of Operation

The ESP32-CSI Tool provides three main operational modes:

- `active_sta`: The ESP32 connects to a router as a client and captures CSI from packets it receives.
- `active_ap`: The ESP32 acts as an access point and collects CSI from devices transmitting to it.
- `passive`: The ESP32 operates in a passive listening mode and captures CSI from all frames it overhears on a given Wi-Fi channel.

These modes are available as separate directories in the project and should be selected based on the use case.

## Steps Involved in CSI Collection

1. **Firmware Setup**: Build and flash the CSI-enabled firmware to the ESP32.
2. **Environment Configuration**: Set Wi-Fi channel, data rate, and CSI output format using `menuconfig`.
3. **Receiving Packets**: ESP32 receives Wi-Fi frames continuously on the configured channel.
4. **CSI Extraction**: For each frame received, the ESP32 extracts CSI including amplitude and phase per subcarrier.
5. **UART Output**: The processed CSI data is sent to the host machine via UART.
6. **Data Logging**: The host system logs the CSI stream using `idf.py monitor` or a custom serial script.

## Important Configuration Parameters (via `menuconfig`)

- Channel number (fixed to avoid channel hopping)
- MAC filter (optional: restrict to packets from a specific transmitter)
- Data rate (fixed for consistency in analysis)
- Console baud rate (921600 or higher recommended)
- FreeRTOS tick rate (set to 1000 Hz)

These parameters are critical to ensure stable, consistent CSI output for post-processing.

## Output Format and Logging

CSI data is transmitted as a structured string with metadata (MAC, RSSI, rate, etc.) and subcarrier values. It is logged in plain text or CSV format and optionally piped through timestamping and plotting utilities for further analysis.

For additional details, refer to the official documentation at: [ESP32-CSI Tool](https://stevenmhernandez.github.io/ESP32-CSI-Tool/)

