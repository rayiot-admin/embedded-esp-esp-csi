# Overview of the ESP32-CSI Tool

The **ESP32-CSI Tool** is an open source project developed to extract
**Channel State Information (CSI)** from Wi-Fi packets received by
ESP32-based devices. CSI provides detailed insights into how a wireless
signal travels from a transmitter to a receiver. It captures phenomena
such as reflection, scattering, fading, and attenuation across individual
subcarriers of the Wi-Fi channel. Compared to traditional metrics like RSSI
(Received Signal Strength Indicator), CSI offers a much more granular view
of the wireless environment.

The **ESP32** is a low-cost, dual-core microcontroller developed by
**Espressif Systems**, featuring integrated Wi-Fi and Bluetooth capabilities.
Its affordability, low power consumption, and programmability make it a
suitable platform for wireless sensing applications. The ESP32-CSI Tool
leverages low-level access to the Wi-Fi driver, enabling the collection of
CSI data that is typically not exposed to the application layer.

The primary goal of the ESP32-CSI Tool is to facilitate real-time CSI data
collection from Wi-Fi frames. This is achieved by:

- Enabling monitor mode on the ESP32  
- Filtering frames by MAC address or type  
- Setting fixed data rates  
- Extracting CSI values for each received frame  

The collected data is transmitted via the serial interface (UART), where it
can be captured, logged, and analyzed using external tools. The tool supports
output formats such as **CSV** and **PCAP**, making it compatible with
popular tools like **Wireshark** or **Python-based data analysis libraries**.

This capability enables a wide range of applications, such as:

- Breathing rate estimation
- Indoor localization and presence detection  
- Gesture recognition without cameras  
- Human activity monitoring for smart environments  
- Research in wireless signal processing and machine learning  

The tool has gained popularity among researchers, engineers, and hobbyists
due to its:

- Open-source nature  
- Minimal hardware requirements (just ESP32 and a computer)  
- Ease of use and extensibility  

The ESP32-CSI Tool was originally developed by **Steven M. Hernandez** and is
actively maintained on GitHub. The project is continually evolving with
contributions from a global community applying it in various domains,
including human-computer interaction, biomedical sensing, and intelligent
monitoring systems.

This documentation provides a comprehensive guide to working with the
ESP32-CSI Tool. It includes:

- Hardware and software setup instructions  
- Step-by-step usage guide  
- CSI data visualization and interpretation techniques  
- Challenges and limitations encountered during implementation  
- Potential improvements and future directions  

The content is enriched with real setup images, sample output screenshots,
graphs, screen recordings, and code examples to support a deeper understanding
of the tool’s functionality and its applications.

