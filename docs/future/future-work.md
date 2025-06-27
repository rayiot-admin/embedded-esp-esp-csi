# Future Work

Although the current implementation of CSI-based sensing using the ESP32-CSI Tool shows promising results for tasks like presence detection and breathing rate estimation, there remain several directions for further improvement and expansion.

---

## 1. Multi-Person Detection and Tracking
- Extend system capabilities to detect and distinguish multiple people in a room
- Requires multiple RX nodes and sophisticated phase/amplitude separation techniques
- Potential use of triangulation and heatmap generation

---

## 2. Integration with Machine Learning
- Train ML models (e.g., SVM, CNN, LSTM) on cleaned amplitude/phase features
- Enable classification tasks such as motion type, activity recognition, or posture detection
- Incorporate real-time inference on edge or cloud platforms

---

## 3. Real-Time Dashboard and Visualization
- Build a live dashboard to display CSI plots, alerts, and inferences
- Use web frameworks (Flask, Dash, or Node.js) to stream data from ESP32 to browser
- Add overlays for BrPM, presence confidence, and subcarrier health

---

## 4. Android and Mobile Integration
- Investigate feasibility of receiving CSI from Android Wi-Fi chipsets directly
- If not possible, build apps that visualize CSI-based outputs from ESP32
- Sync app display with real-time UART or MQTT feed

---

## 5. Single-Device CSI Collection
- Explore using a single ESP32 device in `active_sta` mode connected to a Wi-Fi router to collect CSI data
- This eliminates the need for a second transmitter or receiver
- Useful in compact or mobile setups where hardware footprint must be minimized

---

## 6. Long-Term Stability Studies
- Evaluate CSI stability across hours/days to quantify drift
- Record environmental changes (e.g., time of day, human activity, temperature)
- Apply adaptive calibration methods to improve robustness

---

## 7. Miniaturization and Embedded Deployment
- Port system to low-power or battery-operated ESP32 variants
- Investigate possibility of standalone SD logging with compact form factor
- Integrate into IoT frameworks for scalable deployments

---

## Summary

The ESP32-CSI Tool lays the foundation for a wide range of contactless sensing applications. With further research, integration, and validation, it can support advanced smart home, healthcare, and security systems. Prioritizing machine learning, mobile deployment, and real-time interfaces will unlock its full potential.

