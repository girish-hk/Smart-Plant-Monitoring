# Smart Plant Monitoring — IoT Irrigation System

An IoT-enabled irrigation monitoring system that uses an **ESP32** to read soil moisture and temperature, sends data to a Blynk dashboard, and automates watering. A 2-week pilot showed an approximate **25% improvement in water efficiency**.

---

## Key Features
- Real-time soil moisture and temperature monitoring.
- Remote dashboard and alerts via **Blynk**.
- Automatic irrigation control with configurable thresholds.
- Low-power operation and periodic logging for analytics.
- Modular design: breadboard prototype → PCB option.

---

## Technology Stack
- **Microcontroller:** ESP32 (DevKit)
- **Sensors:** Soil moisture sensor (capacitive or resistive), DHT22 (or DS18B20) for temperature
- **Cloud/UI:** Blynk (mobile dashboard)
- **Languages & Tools:** C/C++ (Arduino / PlatformIO), KiCad (optional for PCB), Git/GitHub

---

## System Overview
- Sensors measure soil moisture and temperature.
- ESP32 samples sensors every configurable interval and sends readings to Blynk.
- If moisture falls below a set threshold, the ESP32 activates a relay (water pump/valve).
- Data is logged locally (optional SD/EEPROM) and can be exported for analysis.

---

## Project Structure
smart-plant-monitoring/
│
├── hardware/
│ ├── schematics/ # KiCad or Fritzing files & images
│ ├── bom.csv # Bill of Materials
│
├── firmware/
│ ├── platformio.ini # PlatformIO config (optional)
│ ├── src/
│ │ └── main.ino # Arduino / ESP32 firmware
│
├── docs/
│ ├── photos/ # Photos: setup, wiring, PCB
│ ├── dashboard_gif.gif # Short demo GIF of Blynk dashboard
│ └── data/ # sample CSVs with pilot data
│
├── .gitignore
├── README.md
└── LICENSE

---

## Results (Pilot)
- **~25% reduction** in water usage during a 2-week pilot on a 10-plant testbed (baseline vs. automated watering).
- Stable sensor readings with ±5% variance after calibration.
- Reliable automatic control with zero reported overwatering incidents.

Include a small CSV or chart in `docs/data/` showing baseline vs pilot daily water usage.

---

## How to Build & Run

### Hardware (quick)
1. Connect soil moisture sensor to an analog input (e.g., `A0` / `GPIO34` on ESP32).
2. Connect temperature sensor (DHT22 or DS18B20) to a digital pin.
3. Use a relay module (driven by a GPIO pin via transistor if necessary) to control the pump/valve.
4. Power ESP32 with 5V (Vin) or USB, and power the pump separately with appropriate supply and common ground.



