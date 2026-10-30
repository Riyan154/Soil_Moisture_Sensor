# 💧 AquaSaver – Smart and Automated Plant Irrigation System

An automated irrigation system designed to conserve water through intelligent soil moisture monitoring and sensor-based watering.

---

## 🌱 About the Project

AquaSaver is a smart irrigation solution developed to reduce water wastage and improve the efficiency of plant watering. The system continuously monitors the moisture levels in the soil using a soil moisture sensor and automatically determines when a plant requires water.

When the soil becomes sufficiently dry, the system activates a water pump through a relay module, supplying water to the plant. Once the soil reaches the required moisture level, the pump automatically stops, preventing unnecessary watering.

In addition to soil moisture monitoring, AquaSaver incorporates a DHT11 sensor to measure the surrounding air temperature and humidity. A 16×2 LCD display provides real-time information about these environmental conditions and the current soil status. Three LEDs provide a simple visual indication of whether the soil is dry, getting dry, or sufficiently moist.

The project also incorporates capacitors and diodes as part of its electronic circuitry to support the operation and protection of the circuit.

By combining automated irrigation, environmental monitoring, and real-time feedback, AquaSaver aims to promote responsible water consumption and provide a foundation for more efficient irrigation systems.

---

## 🎯 Objectives

- Conserve Water: Reduce unnecessary water consumption by supplying water only when required.
- Automate Irrigation: Eliminate the need for continuous manual monitoring and watering.
- Monitor Soil Conditions: Measure soil moisture levels to determine the watering requirements of plants.
- Track Environmental Conditions: Monitor surrounding temperature and humidity.
- Provide Real-Time Feedback: Display sensor readings and soil conditions through an LCD and LED indicators.
- Promote Sustainability: Encourage efficient water management through affordable automation.

---

## ⚙️ How It Works

AquaSaver operates through a continuous cycle of soil monitoring and automated irrigation.

1. Soil Monitoring: The soil moisture sensor measures the moisture level present in the soil and sends the readings to the Arduino Uno.

2. Data Processing: The Arduino processes the readings and categorises the soil condition into three states: Dry, Getting Dry, and Moist.

3. Automatic Watering: When the soil becomes sufficiently dry, the Arduino activates the relay module, which switches on the water pump.

4. Moisture Regulation: As the soil receives water and reaches the required moisture threshold, the system switches off the pump.

5. Environmental Monitoring: The DHT11 sensor measures the surrounding temperature and humidity.

6. Output Display: The LCD displays the soil condition, temperature, and humidity, while the LEDs indicate the current soil moisture status.

---

## 🔧 Components Used

| Component | Purpose |
|---|---|
| Arduino Uno R3 | Main microcontroller that processes sensor readings and controls the system |
| Soil Moisture Sensor | Measures the moisture content of the soil |
| DHT11 Sensor | Measures surrounding temperature and humidity |
| Relay Module | Controls the water pump |
| 5V Water Pump | Supplies water to the plant |
| 16×2 LCD Display | Displays soil status, temperature, and humidity |
| Green LED | Indicates sufficiently moist soil |
| Yellow LED | Indicates soil that is getting dry |
| Red LED | Indicates dry soil |
| Resistors | Limit current and help protect electronic components |
| Capacitors | Help stabilise the power supply and reduce electrical noise |
| Diodes | Provide electrical protection depending on their circuit placement |
| Breadboard | Provides connections between components |
| Jumper Wires | Establish electrical connections |

---

## 💻 Technologies Used

- Programming Language: C++ (Arduino)
- Microcontroller: Arduino Uno R3
- Development Environment: Arduino IDE
- Libraries: DHT Sensor Library and LiquidCrystal Library

---

## 📟 System Features

### 1. Automated Irrigation

The system automatically activates the water pump when the soil moisture falls below the defined dry-soil threshold. It stops watering when the required moisture condition is reached.

### 2. Three-Level Soil Monitoring

The system classifies soil conditions into three categories:

- 🔴 Dry: The soil requires watering.
- 🟡 Getting Dry: The soil moisture is approaching the watering threshold.
- 🟢 Moist: The soil has sufficient moisture, and watering is stopped.

### 3. Temperature and Humidity Monitoring

The DHT11 sensor continuously measures the surrounding air temperature and relative humidity, providing additional environmental information.

### 4. LCD Monitoring

A 16×2 LCD displays the current soil condition on the first row and temperature and humidity readings on the second row.

### 5. Stable Sensor Readings

The system averages multiple soil sensor readings and requires consistent readings before changing the soil status. This helps reduce sudden fluctuations caused by inconsistent sensor measurements.

---

## 📸 Project Gallery

### Project Setup

Add photographs of the complete AquaSaver prototype, including the Arduino, sensors, LCD, relay module, capacitors, diodes, and water pump.

<!-- Add your project setup photograph here -->

### Hardware Components

Add individual photographs showing the main hardware components and their connections.

<!-- Add your hardware photographs here -->

### LCD Display

Show the LCD displaying the soil moisture condition, temperature, and humidity.

<!-- Add your LCD photographs here -->

### Automatic Watering Demonstration

Show the water pump operating when the soil moisture reaches the dry threshold.

<!-- Add your water pump photographs here -->

---

## 🎥 Project Demonstration

Add a video demonstrating the complete working of AquaSaver.

Team Photo:

<img width="1280" height="853" alt="WhatsApp Image 2026-10-30 at 2 03 20 PM" src="https://github.com/user-attachments/assets/68024112-5554-4e2a-a184-3717d16efff6" />

<img width="1280" height="853" alt="WhatsApp Image 2026-10-30 at 2 03 20 PM" src="https://github.com/user-attachments/assets/ec898f68-8aad-44f6-a4ff-78ca2b523ad0" />


Demonstration Video:

With my Team:

https://github.com/user-attachments/assets/d28bb64f-6d02-4b3b-9898-f2b034407e31

https://github.com/user-attachments/assets/8c0dddb1-938f-49a5-9477-9e4798f1826a

Model Photos:

<img width="1280" height="853" alt="WhatsApp Image 2026-10-30 at 2 03 20 PM (2)" src="https://github.com/user-attachments/assets/c07df15f-3acd-44cd-aa2a-a03109af09c0" />
<img width="1280" height="853" alt="WhatsApp Image 2026-10-30 at 2 03 20 PM (1)" src="https://github.com/user-attachments/assets/e723743c-84bd-4bc1-859c-8ab0d17c1e0c" />




The demonstration should showcase:

- Soil moisture sensor readings.
- LED indicators changing according to soil conditions.
- Automatic activation of the water pump.
- Pump stopping when the required moisture level is reached.
- LCD temperature and humidity readings.

---

## 🔮 Future Improvements

Although AquaSaver currently operates as an automated plant irrigation prototype, several improvements could expand its capabilities:

- Multiple Soil Sensors: Integrate additional moisture sensors to monitor different areas of a garden or agricultural field.
- Larger-Scale Irrigation: Extend the system to support multiple plants and irrigation zones.
- Water Flow Monitoring: Incorporate a flow sensor to measure the quantity of water supplied to plants.
- Wireless Monitoring: Introduce Wi-Fi connectivity to monitor soil conditions remotely through a mobile application or web dashboard.
- Water Usage Analysis: Record water consumption data to analyse irrigation patterns and identify opportunities for further conservation.
- Solar Power Integration: Explore solar energy as an alternative power source to improve energy efficiency.

---

## 🌍 Environmental Impact

Water is an essential resource, and inefficient irrigation can lead to unnecessary water consumption. AquaSaver addresses this issue by introducing automation into the plant watering process.

Instead of watering plants according to a fixed schedule, the system makes watering decisions based on actual soil moisture readings. This approach can help reduce unnecessary irrigation, minimise manual effort, and encourage more responsible water management.

The project's underlying concept can be further developed and adapted for gardens, green spaces, and agricultural applications.

---

## 👥 Project Information

Project Name: AquaSaver

Category: Robotics, Embedded Systems and Smart Irrigation

Microcontroller: Arduino Uno R3

Primary Objective: Water Conservation and Automated Irrigation

Project Type: Group Project

Developer: Riyan, Reyhan, Basim, Blesson and Rehan

---

## ⭐ Acknowledgements

AquaSaver was developed as a collaborative group project exploring Arduino-based automation, sensor integration, and sustainable water management.

Through teamwork, experimentation, and practical implementation, we combined hardware and software to demonstrate how accessible technology can contribute to solving everyday environmental challenges.
