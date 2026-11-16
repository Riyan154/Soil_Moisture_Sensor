<div align="center">

# 💧 AquaSaver
### Smart & Automated Plant Irrigation System

**Every drop matters. Every plant deserves the right amount of water.**

An Arduino-powered prototype that monitors soil moisture, reports environmental conditions, and controls irrigation through a relay-driven water pump.

[![Arduino](https://img.shields.io/badge/Arduino-Uno_R3-00878F?style=for-the-badge&logo=arduino&logoColor=white)](https://www.arduino.cc/)
![Language](https://img.shields.io/badge/Language-Arduino_C%2B%2B-2563EB?style=for-the-badge)
![Project](https://img.shields.io/badge/Project-Smart_Irrigation-15803D?style=for-the-badge)
![Focus](https://img.shields.io/badge/Focus-Water_Conservation-0891B2?style=for-the-badge)

**Monitor soil · Understand conditions · Automate watering · Conserve resources**

</div>

---

## 🌱 The idea

Traditional watering often relies on fixed schedules or guesswork. But plants do not always need water at the same time: their needs change with soil conditions and the surrounding environment.

**AquaSaver** explores a more responsive approach. An **Arduino Uno R3** reads a soil-moisture sensor, assigns the soil to one of three moisture states, displays feedback on a **16×2 LCD**, lights a matching **LED**, and sends a control signal to a **relay-connected 5V pump**. A **DHT11** also provides ambient temperature and humidity readings.

The goal is simple: **water when the soil needs it, rather than watering unnecessarily.**

> AquaSaver is an educational working prototype, not a certified irrigation controller. Its readings and thresholds must be calibrated for the actual soil, plant, sensor, and hardware.

## ✨ What makes AquaSaver useful?

| Capability | What it does |
| :--- | :--- |
| 💧 **Soil monitoring** | Converts analog sensor readings into an estimated moisture percentage |
| 🚦 **Clear visual status** | Uses green, yellow, and red LEDs to indicate soil condition |
| 🚿 **Pump control** | Uses an Arduino-controlled relay to switch a small water pump |
| 🌡️ **Environment sensing** | Displays ambient temperature and relative humidity from a DHT11 |
| 📟 **Instant feedback** | Shows soil status and environmental readings on a 16×2 LCD |
| 📈 **Serial diagnostics** | Reports sensor status and readings through the Arduino Serial Monitor |
| 🧠 **Reading stabilization** | Averages sensor samples and waits for consistent state readings |

## ⚙️ How the system is designed to work

```text
           ┌───────────────────┐
           │ Soil moisture     │
           │ sensor (A0)       │
           └─────────┬─────────┘
                     │
                     v
           ┌───────────────────┐
           │ Arduino Uno R3    │<──── DHT11 (D7)
           │                   │
           │ • Read sensors    │
           │ • Estimate %      │
           │ • Classify soil   │
           └─────────┬─────────┘
                     │
          ┌──────────┼───────────┐
          v          v           v
      LEDs       16×2 LCD     Relay (D13)
   Red/Yellow/   Soil status      │
      Green      Temp/Humidity    v
                              5V water pump
```

**Intended operating cycle:**

1. **Measure:** The moisture probe reads the soil and sends an analog value to the Arduino.
2. **Smooth:** The program averages five samples to reduce short-term noise.
3. **Classify:** The estimated moisture percentage determines whether the soil is dry, getting dry, or moist.
4. **Indicate:** An LED and LCD communicate the current state.
5. **Water:** The intended controller turns the pump on for dry soil and off when adequate moisture is restored.
6. **Repeat:** The Arduino continuously checks the environment and updates the outputs.

**Important implementation note:** The checked-in sketch currently has a **relay polarity mismatch**. The comment identifies the relay as active-low, but `controlPump()` writes `HIGH` in the dry state and `LOW` otherwise. For an active-low relay, those commands produce the **opposite** of the intended behavior. See [Known limitations](#-known-limitations) before connecting or operating the pump. This README does not modify the sketch.

## 🚦 Soil moisture states

These are the **actual thresholds in the current Arduino code**, using its calibrated estimate of moisture percentage:

| Condition | Estimated soil moisture | Indicator | Intended pump behavior |
| --- | ---: | :---: | --- |
| **Dry** | Below 69% | 🔴 Red | Water the plant |
| **Getting dry** | 69% to below 85% | 🟡 Yellow | Pump off |
| **Moist** | 85% or higher | 🟢 Green | Pump off |

The percentages depend on sensor calibration and should **not** be interpreted as universal agricultural soil-moisture measurements.

### Calibration used in the sketch

```cpp
int moisturePercent = map(soilValue, 1023, 250, 0, 100);
moisturePercent = constrain(moisturePercent, 0, 100);
```

| Reading | Calibration meaning |
| --- | --- |
| **1023** | Dry-end reference (mapped to 0%) |
| **250** | Wet-end reference (mapped to 100%) |
| **85%** | Green-state threshold |
| **69%** | Yellow-state threshold |

The program requires **two consecutive readings** supporting a new moisture state before changing the current state. Each reading is itself an average of five analog measurements.

## 🧰 Hardware components

| Component | Purpose |
| --- | --- |
| **Arduino Uno R3** | Processes the sensor readings and controls outputs |
| **Analog soil moisture sensor** | Estimates soil wetness |
| **DHT11 sensor** | Measures ambient temperature and humidity |
| **16×2 LCD display** | Displays soil condition, temperature, and humidity |
| **5V water pump** | Delivers water to the plant |
| **Relay module** | Switches the pump circuit |
| **Green, yellow, and red LEDs** | Show the current soil condition |
| **Current-limiting resistors** | Protect the LEDs |
| **Breadboard and jumper wires** | Provide prototype connections |
| **External 5V pump supply / breadboard power module** | Provides a separate source of power for the pump |
| **Flyback diode** | Helps suppress motor-switching voltage transients |
| **Decoupling / bulk capacitors** | Help reduce electrical noise and supply dips |

The prototype uses low-voltage electronics. Pump power requirements must be checked against the external supply's current capability; a breadboard power module should not be assumed to support every pump. Keep the electronics protected from water and do not power the pump directly from an Arduino output pin.

## 🔌 Arduino pin reference

The following assignments are taken directly from `SoilMoistureSensor.ino`.

| Part | Arduino connection |
| --- | --- |
| Soil moisture sensor (analog output) | **A0** |
| DHT11 data | **D7** |
| Green LED | **D8** |
| Yellow LED | **D9** |
| Red LED | **D10** |
| Relay signal | **D13** |
| LCD RS | **D12** |
| LCD Enable | **D11** |
| LCD D4 | **D5** |
| LCD D5 | **D4** |
| LCD D6 | **D3** |
| LCD D7 | **D2** |

**Power and grounding:** Use correct power rails for each component and a suitable pump supply. Logic-side grounds must be referenced appropriately for the relay input. Follow the specific relay, sensor, and power module documentation rather than assuming their terminals behave identically.

## 💻 Software and libraries

- **Language:** Arduino C++
- **Board:** Arduino Uno R3
- **Editor:** Arduino IDE
- **Libraries:** `DHT.h` (DHT sensor library) and `LiquidCrystal.h` (LCD interface)
- **Monitoring:** Arduino Serial Monitor at **9600 baud**

**Source file:** [`SoilMoistureSensor.ino`](SoilMoistureSensor.ino)

## 🚀 Getting started

1. **Clone or download** this repository.
   ```bash
   git clone https://github.com/Riyan154/Soil_Moisture_Sensor.git
   ```
2. **Open** `SoilMoistureSensor.ino` in the Arduino IDE.
3. **Install** the Adafruit DHT sensor library and its required dependencies through Library Manager if they are not already installed. The standard `LiquidCrystal` library is also required.
4. **Select** the correct Arduino Uno board and USB port.
5. **Inspect and correct the relay logic for your actual relay module before operating the pump.** The checked-in code contains the mismatch described above.
6. **Connect** your components according to the pin reference, checking supply polarity, current ratings, and water isolation.
7. **Upload** the sketch and open Serial Monitor at **9600 baud**.
8. **Calibrate** the moisture readings for your sensor and growing medium, then test each state before leaving the system running.

> ⚠️ **Safety:** Disconnect power before changing wiring. Use a suitable low-voltage external pump supply, account for motor back-EMF, and keep water away from exposed electrical connections. This prototype does not implement a water-level cutoff or a maximum pump-runtime failsafe.

## 📟 What you will see

### LCD display

The first LCD row shows the soil condition. The second row shows ambient temperature and humidity.

```text
Soil:MOIST
T:25C H:60%
```

*Illustrative display format; the actual readings depend on your environment and sensor.*

### Serial Monitor

The sketch also prints the soil state and temperature/humidity data through the serial connection. When the DHT11 fails to provide valid readings, it displays a **DHT11 read error** instead of fabricated values.

## 🔬 Engineering decisions

**Why average readings?** Analog soil sensors can fluctuate. Taking five samples helps smooth brief disturbances and improves the stability of the displayed value.

**Why wait for consistent states?** Requiring two consistent classifications helps prevent rapid LED and pump-state changes at a threshold boundary, although it is not a full hysteresis controller.

**Why use a relay?** The microcontroller can control a separate pump circuit without attempting to supply motor current through a digital I/O pin.

**Why use a separate pump supply and protective components?** Small motors can generate electrical noise and supply disturbances that interfere with LCDs and microcontrollers. Proper separation, protection, and decoupling improve reliability.

## 🌍 Impact and practical value

AquaSaver demonstrates the concept of **sensor-based irrigation**: respond to measured conditions rather than watering on a fixed timetable. With careful calibration, appropriate safeguards, and field testing, this approach has the potential to reduce unnecessary watering and manual monitoring.

The current prototype does **not** measure actual water savings or guarantee optimum plant health. Those outcomes would need comparison trials, water-flow measurements, and crop-specific calibration.

## 🧩 Known limitations

| Limitation | What needs improvement |
| --- | --- |
| **Relay polarity mismatch in the current sketch** | Match the pump-control outputs to the relay's real active level; verify dry = ON and moist = OFF |
| **Single-point soil measurement** | Add multiple calibrated sensors for different beds or zones |
| **Raw moisture percentage is sensor-specific** | Calibrate in the target soil instead of assuming the same dry/wet values everywhere |
| **Fixed moisture thresholds** | Tune thresholds for different plants, soils, and conditions |
| **No maximum pump-run safeguard** | Add timeout, dry-run / tank-level protection, and fault handling |
| **No measured water-consumption feedback** | Add a flow sensor to quantify delivery and water savings |
| **Potential corrosion in some soil probes** | Consider more durable capacitive probes for long-term deployment |
| **Prototype wiring and pump interference** | Use reliable connectors, protected enclosures, stable supplies, and appropriate motor suppression |

## 🔭 Future possibilities

- **Multi-zone irrigation:** Monitor several plants or garden sections independently.
- **Flow and tank monitoring:** Measure water delivered and detect low-water conditions.
- **Smarter control:** Introduce hysteresis, maximum run time, sensor-fault handling, and per-plant settings.
- **Remote monitoring:** Add Wi-Fi connectivity and an optional dashboard for alerts and trends.
- **Energy-conscious operation:** Evaluate a properly sized battery or solar-powered supply.
- **Long-term data analysis:** Track moisture, pump activity, and actual water consumption over time.

## 👥 Project team

**AquaSaver** is a collaborative Arduino and sustainability project.

**Team members:** Riyan · Reyhan · Basim · Blesson · Rehan

**Project areas:** Robotics, embedded systems, environmental monitoring, and water conservation.

## 🤝 Acknowledgements

This project brought together programming, electronics, prototyping, sensor calibration, and teamwork to explore an everyday environmental challenge. AquaSaver shows how accessible components can become a foundation for practical, responsible automation.

---

<div align="center">

### 💧 Smarter watering starts with better information.

**AquaSaver — Monitor wisely. Water responsibly.**

[**Explore the Arduino code**](SoilMoistureSensor.ino) · [**Back to repository**](https://github.com/Riyan154/Soil_Moisture_Sensor)

</div>
