# 🍳 Smart Cooktop Control System: IoT-Enabled Automated Kitchen

<p align="center">

**Embedded Systems | ESP32 | IoT | Sensor Fusion | Kitchen Automation | Safety Control**

</p>

<p align="center">
  <img src="https://img.shields.io/badge/Domain-Embedded%20Systems-blue" alt="Embedded Systems">
  <img src="https://img.shields.io/badge/Platform-ESP32-orange" alt="ESP32">
  <img src="https://img.shields.io/badge/Language-Embedded%20C-green" alt="Embedded C">
  <img src="https://img.shields.io/badge/Application-Smart%20Kitchen-red" alt="Smart Kitchen">
  <img src="https://img.shields.io/badge/Control-Automation-purple" alt="Automation">
</p>

---

## 📌 Overview

The **Smart Cooktop Control System** is an IoT-enabled automated kitchen prototype designed to improve **cooking safety, efficiency, temperature control, and user convenience**.

The system upgrades a conventional cooktop by integrating an **ESP32 microcontroller**, ultrasonic sensing, infrared temperature monitoring, automated stirring, relay-based cooktop control, and an OLED interface.

The system continuously monitors both the **cooking vessel and its contents**. It detects potential overflow conditions using an ultrasonic sensor and monitors cooking temperature using an infrared temperature sensor.

When unsafe conditions are detected, the ESP32 automatically responds by activating a buzzer and/or cutting power to the induction cooktop through a relay.

At the same time, a motor-driven stirring mechanism periodically mixes the contents to provide more uniform cooking.

```text
Sensors
   ↓
ESP32
   ↓
Decision Logic
   ├── Overflow Detection
   ├── Temperature Monitoring
   └── Stirring Control
   ↓
Actuators
   ├── Relay
   ├── Buzzer
   └── DC Motor
   ↓
OLED Feedback
```

Developed as part of the **Microprocessors and Microcontrollers course at VIT Chennai**.

---

# 🎯 Project Objectives

The primary objectives of the system are:

1. Detect and prevent cooking vessel overflow.
2. Continuously monitor cooking temperature.
3. Automatically shut down heating when unsafe temperatures are detected.
4. Automate stirring at programmed intervals.
5. Monitor utensil and food height using ultrasonic sensing.
6. Provide real-time cooking information through an OLED display.
7. Improve cooking consistency through automated control.
8. Reduce food wastage caused by boil-over incidents.
9. Improve kitchen safety through automatic intervention.
10. Demonstrate practical integration of sensors, actuators, and embedded control using an ESP32.

---

# ⭐ Key Features

### 🌊 Real-Time Overflow Detection

An **HC-SR04 ultrasonic sensor** monitors the height of the vessel contents.

When the detected food level approaches a predefined threshold, the system identifies a potential overflow condition.

The system can then:

* Activate the buzzer
* Toggle the relay
* Cut power to the induction cooktop

---

### 🌡️ Temperature Monitoring

An **Adafruit MLX90614 infrared temperature sensor** continuously monitors cooking temperature.

If the measured temperature exceeds the configured threshold, the ESP32 automatically disables the cooktop through the relay.

Example safety threshold:

```text
Temperature > 80°C
        ↓
Safety Trigger
        ↓
Relay OFF
        ↓
Heating Disabled
```

---

### 🥄 Automated Stirring

A DC motor-driven mechanical stirrer automatically mixes the contents at programmed intervals.

```text
ESP32
  │
  ▼
L298N Motor Driver
  │
  ▼
DC Motor
  │
  ▼
Mechanical Stirrer
  │
  ▼
Uniform Mixing
```

This reduces the requirement for continuous manual stirring and helps maintain more consistent cooking.

---

### 📏 Smart Utensil Height Sensing

The ultrasonic sensor is also used to estimate the height of the vessel contents.

The system uses the relationship between:

```text
Sensor → Vessel Surface
```

and

```text
Sensor → Food Surface
```

to determine the approximate food level.

This allows the system to identify when the contents are approaching the vessel's overflow limit.

---

### 🖥️ Interactive OLED Display

A **0.96-inch SSD1306 OLED** provides real-time feedback.

The display can show information such as:

* Temperature
* Utensil/food height
* Overflow status
* System state
* Cooking conditions

This allows the user to monitor the system without interacting directly with the hardware.

---

# 🏗️ System Architecture

The complete system consists of a sensing layer, control layer, actuation layer, and user-interface layer.

```text
                    ┌─────────────────────┐
                    │      HC-SR04        │
                    │ Ultrasonic Sensor   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │      MLX90614       │
                    │ IR Temperature      │
                    │      Sensor         │
                    └──────────┬──────────┘
                               │
                               ▼
                     ┌──────────────────┐
                     │      ESP32       │
                     │                  │
                     │ Sensor Reading   │
                     │ Decision Logic   │
                     │ Safety Control   │
                     │ Stirring Control │
                     └───────┬──────────┘
                             │
               ┌─────────────┼─────────────┐
               │             │             │
               ▼             ▼             ▼
         ┌──────────┐   ┌──────────┐  ┌──────────┐
         │  Relay   │   │  Buzzer  │  │   L298N  │
         └────┬─────┘   └──────────┘  └────┬─────┘
              │                             │
              ▼                             ▼
       Induction Cooktop              DC Stirrer
              
                     ┌──────────────────┐
                     │      OLED        │
                     │   SSD1306       │
                     └──────────────────┘
```

---

# 🔄 Complete System Workflow

The system continuously executes the following sequence:

```text
             START
               │
               ▼
        Initialize Sensors
               │
               ▼
       Read Temperature
               │
               ▼
        Read Food Height
               │
               ▼
       ┌───────┴────────┐
       │                │
       ▼                ▼
 Temperature        Overflow
   Check              Check
       │                │
       └───────┬────────┘
               ▼
         Decision Logic
               │
      ┌────────┼────────┐
      ▼        ▼        ▼
   Normal   Warning   Shutdown
      │        │        │
      │        ▼        ▼
      │      Buzzer    Relay OFF
      │
      ▼
 Scheduled Stirring
      │
      ▼
   OLED Update
      │
      ▼
    Repeat
```

---

# 🌊 Overflow Detection

Overflow prevention is one of the primary safety functions.

The HC-SR04 measures the distance between the sensor and the cooking surface.

```text
       HC-SR04
          │
          ▼
     ───────────
       Sensor
     ───────────
          │
          │ Distance
          ▼
      ~~~~~~~~~~~
      Food Level
      ~~~~~~~~~~~
          │
          ▼
        Vessel
```

As the food level rises, the measured distance decreases.

The controller can therefore identify when the contents approach the predefined overflow region.

---

# 🚨 Overflow Response

When a potential overflow is detected:

```text
High Food Level
      ↓
Overflow Threshold
      ↓
ESP32 Detection
      ↓
┌─────┴──────┐
▼            ▼
Buzzer      Relay
ON          OFF
             │
             ▼
      Cooktop Disabled
```

This provides an automatic safety response rather than relying entirely on user intervention.

---

# 🌡️ Temperature Control

The MLX90614 infrared temperature sensor allows **non-contact temperature measurement**.

The measured temperature is continuously compared against a predefined threshold.

For example:

```text
Measured Temperature
        │
        ▼
     > 80°C ?
     /      \
   YES       NO
    │         │
    ▼         ▼
Relay OFF   Continue
    │
    ▼
Heating Disabled
```

The system is therefore capable of automatically responding to overheating conditions.

---

# 🔌 Relay-Based Cooktop Control

The relay provides the interface between the low-voltage embedded controller and the cooktop's power-control path.

```text
ESP32
  │
  ▼
Control Signal
  │
  ▼
Relay Module
  │
  ▼
Cooktop Power
```

When a safety condition is detected, the ESP32 switches the relay to disable heating.

> ⚠️ Any real mains-voltage implementation requires appropriate isolation, enclosure, fusing, wiring, and electrical-safety practices. The prototype should be treated as a controlled academic system rather than a certified appliance controller.

---

# 🥄 Automated Stirring System

The automated stirrer is driven using:

* DC motor
* L298N motor driver
* Mechanical stirring mechanism

The ESP32 controls the motor according to the programmed stirring schedule.

```text
        ESP32
          │
          ▼
     Motor Control
          │
          ▼
       L298N
          │
          ▼
       DC Motor
          │
          ▼
      Stirrer
          │
          ▼
   Uniform Mixing
```

This is particularly useful for recipes requiring frequent or continuous stirring.

---

# 🖥️ OLED Interface

The OLED serves as the primary real-time user feedback interface.

Conceptually, the display can present:

```text
┌──────────────────────┐
│   SMART COOKTOP      │
├──────────────────────┤
│ Temp:     76.8 °C    │
│ Height:    4.2 cm    │
│ Overflow:    SAFE    │
│ Stirring:    ON      │
└──────────────────────┘
```

This gives the user a quick overview of the current cooking state.

---

# 🧠 Embedded Control Logic

The ESP32 acts as the central controller.

Its responsibilities include:

* Sensor acquisition
* Temperature monitoring
* Height measurement
* Overflow detection
* Safety threshold comparison
* Relay control
* Buzzer control
* Motor control
* OLED updates
* System calibration

The architecture follows a **closed-loop embedded control approach**:

```text
        Physical System
              │
              ▼
           Sensors
              │
              ▼
            ESP32
              │
        Decision Logic
              │
              ▼
          Actuators
              │
              ▼
        Physical System
              │
              └──────→ Feedback
```

---

# 🛠️ Hardware

| Component              | Function                               |
| ---------------------- | -------------------------------------- |
| **ESP32**              | Main microcontroller and control logic |
| **HC-SR04**            | Ultrasonic height/overflow sensing     |
| **Adafruit MLX90614**  | Infrared temperature measurement       |
| **Relay Module**       | Cooktop power control                  |
| **DC Motor**           | Automated stirring                     |
| **L298N**              | DC motor driver                        |
| **0.96" SSD1306 OLED** | Real-time system display               |
| **Buzzer**             | Overflow/safety warning                |

---

# 💻 Technologies Used

## Microcontroller

**ESP32**

Provides:

* GPIO
* Sensor interfacing
* Control logic
* Motor control
* Display communication

---

## Programming

**Embedded C / Arduino framework**

The firmware is developed for the ESP32 using the Arduino IDE.

---

## Sensors

### HC-SR04

Used for:

* Vessel height measurement
* Food-level estimation
* Overflow prediction

### MLX90614

Used for:

* Non-contact temperature measurement
* Overheating detection
* Temperature-based safety control

---

## Actuators

### Relay

Controls the cooktop's heating power.

### DC Motor + L298N

Controls the automated stirring mechanism.

### Buzzer

Provides an audible warning during unsafe conditions.

---

# 🧰 Development Environment

The system is developed using:

```text
Arduino IDE
+
ESP32 Board Package
+
Embedded C
```

Required libraries include:

```cpp
#include <Wire.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_MLX90614.h>
```

---

# 📁 Suggested Repository Structure

```text
Smart-Cooktop-Control-System/
│
├── README.md
│
├── firmware/
│   └── smart_cooktop.ino
│
├── docs/
│   └── Smart_Cooktop_Project_Report.pdf
│
├── hardware/
│   ├── circuit_diagram.png
│   └── hardware_setup.jpg
│
└── images/
    └── cooktop_prototype.jpg
```

---

# 🚀 Getting Started

## Requirements

### Hardware

* ESP32
* HC-SR04 ultrasonic sensor
* MLX90614 infrared temperature sensor
* Relay module
* DC motor
* L298N motor driver
* SSD1306 OLED
* Buzzer
* Mechanical stirrer
* Induction cooktop/prototype load
* Appropriate power supplies and wiring

### Software

* Arduino IDE
* ESP32 board support
* Required sensor/display libraries
* USB cable
* Correct ESP32 COM port

---

# 🔌 Hardware Assembly

Connect the sensors and actuators to the ESP32 according to the project's circuit design.

The HC-SR04 requires separate trigger and echo connections.

The MLX90614 communicates through the **I²C interface**.

The OLED also communicates through I²C.

The L298N interfaces the ESP32 with the DC motor.

The relay receives the ESP32's control signal for cooktop power switching.

> The exact GPIO assignments should follow the final circuit/firmware implementation rather than being assumed from the README.

---

# 💻 Software Setup

### 1. Install Arduino IDE

Install the Arduino IDE on the development computer.

### 2. Add ESP32 Support

Open:

```text
Tools
→ Board
→ Boards Manager
```

Search for:

```text
ESP32
```

and install the ESP32 board package.

### 3. Install Libraries

Install the required libraries:

```text
Wire
Adafruit SSD1306
Adafruit MLX90614
```

### 4. Select Board

Select:

```text
ESP32 Dev Module
```

### 5. Select COM Port

Choose the serial port corresponding to the connected ESP32.

---

# 📤 Flashing the Firmware

Open the main `.ino` firmware file in Arduino IDE.

Then:

```text
Select Board
      ↓
Select COM Port
      ↓
Compile
      ↓
Upload
      ↓
Open Serial Monitor
```

After successful programming, power the system and verify that the OLED and sensors initialize correctly.

---

# ▶️ Operating the System

The basic operating sequence is:

### Step 1 — Power the System

Power the ESP32 and connected modules.

### Step 2 — Initialize

The system initializes:

* Ultrasonic sensor
* MLX90614
* OLED
* Motor driver
* Relay
* Buzzer

### Step 3 — Calibration

Perform the initial calibration using the available button/control mechanism.

### Step 4 — Begin Monitoring

The ESP32 continuously monitors:

```text
Temperature
+
Food Height
+
Overflow Condition
```

### Step 5 — Automated Control

Depending on the measured conditions, the system can:

* Continue heating
* Activate the buzzer
* Disable the cooktop
* Activate the stirrer

### Step 6 — OLED Feedback

The current system state is displayed on the OLED.

---

# 📊 Project Results

Testing of the prototype reported the following improvements:

| Metric                     |      Reported Result |
| -------------------------- | -------------------: |
| Boil-over incidents        |   **~45% reduction** |
| Kitchen safety             | **~38% improvement** |
| Automated cooking response |       **23% faster** |
| Temperature control        |           **±1.2°C** |

These results demonstrate the potential of combining sensing and automatic control for safer and more consistent cooking.

---

# 📈 Performance Analysis

The system's reported performance can be summarized as:

```text
         Smart Monitoring
                │
        ┌───────┼────────┐
        ▼       ▼        ▼
    Overflow  Thermal   Stirring
    Control   Control   Automation
        │       │        │
        └───────┼────────┘
                ▼
       Improved Automation
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
    Safety   Accuracy  Convenience
```

The prototype reported:

* Approximately **45% fewer boil-over incidents**
* Approximately **38% improvement in kitchen safety**
* **23% faster response** in automated cooking sequences
* Temperature control within approximately **±1.2°C**

---

# 🔍 Verification Strategy

The system can be evaluated through several test categories.

## Overflow Testing

Test different liquid/food levels and verify whether the ultrasonic system detects the predefined threshold.

## Temperature Testing

Compare MLX90614 measurements against reference temperature measurements.

## Relay Testing

Verify that the cooktop is disabled when:

* Overflow is detected
* Temperature exceeds the safety threshold

## Stirring Testing

Verify that the motor activates at the programmed intervals.

## OLED Testing

Confirm that temperature, height, and safety states are correctly displayed.

## Response-Time Testing

Measure the time between:

```text
Unsafe Condition
      ↓
Sensor Detection
      ↓
ESP32 Decision
      ↓
Actuator Response
```

---

# 🧠 Key Concepts Demonstrated

This project provides practical exposure to:

* Embedded systems
* ESP32 programming
* Embedded C
* Microprocessors and microcontrollers
* Sensor interfacing
* Ultrasonic sensing
* Infrared temperature sensing
* I²C communication
* OLED interfaces
* Relay control
* Motor control
* L298N motor driver
* Automated stirring
* Closed-loop control
* Threshold-based decision making
* Real-time monitoring
* Kitchen automation
* IoT-enabled embedded systems
* Safety-oriented embedded design

---

# 💡 What I Learned

The project demonstrates how an embedded controller can integrate **multiple sensors and actuators into a real-time automated control system**.

The overall concept can be summarized as:

```text
        SENSING
           ↓
   ┌───────┴────────┐
   ▼                ▼
Temperature      Food Height
   │                │
   └───────┬────────┘
           ▼
     DECISION LOGIC
           │
     ┌─────┼─────┐
     ▼     ▼     ▼
   Relay Buzzer Motor
     │     │     │
     └─────┼─────┘
           ▼
      COOKING SYSTEM
           │
           ▼
        FEEDBACK
           │
           └────→ Sensors
```

The project highlights an important embedded-systems principle:

> **Sensors provide the information, the microcontroller makes the decision, and actuators physically change the system's behaviour.**

---

# 🏠 Potential Applications

The architecture can be adapted for:

### Smart Kitchens

* Automated cooking assistance
* Smart cooktops
* Automated stirring
* Cooking safety systems

### Food Processing

* Temperature monitoring
* Automated mixing
* Process control

### Domestic Safety

* Overheating prevention
* Overflow detection
* Automatic appliance shutdown

### IoT Appliances

Future connectivity could allow users to:

* Monitor cooking remotely
* Receive alerts
* Control cooking parameters
* Track energy consumption

---

# ⚠️ Limitations

The current prototype has several limitations.

### Sensor Dependence

Ultrasonic measurements can be affected by vessel geometry, food surface characteristics, and sensor placement.

### Infrared Temperature Measurement

The MLX90614 measures infrared radiation and therefore depends on surface/emissivity and measurement geometry.

### Fixed Thresholds

The current system relies on predefined safety thresholds rather than dynamically learning different cooking conditions.

### Single-Utensil Operation

The prototype is designed around monitoring one primary cooking vessel.

### IoT Functionality

Although described as IoT-enabled, the provided implementation focuses primarily on the embedded monitoring and control system; a full cloud/mobile platform would require additional development.

---

# 🚀 Future Scope

The project can be extended through:

## 📱 Mobile Application

Develop a companion application for:

* Remote temperature monitoring
* Overflow alerts
* Cooking-status updates
* Remote control

---

## 🗣️ Voice Assistant Integration

Integration with platforms such as:

* Alexa
* Google Assistant

could allow hands-free cooking interaction.

---

## 🤖 AI-Based Food Recognition

A future computer-vision module could identify the food being cooked and automatically select an appropriate cooking profile.

```text
Camera
  ↓
Food Recognition
  ↓
Food Type
  ↓
Cooking Profile
  ↓
Temperature + Stirring Control
```

---

## ⚡ Energy Monitoring

Add current/voltage sensing to estimate:

* Energy consumption
* Cooking efficiency
* Power usage per recipe

---

## 🍲 Multi-Utensil Management

A more advanced system could independently monitor multiple vessels:

```text
              ESP32
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
    Vessel 1 Vessel 2 Vessel 3
       │        │        │
   Sensors   Sensors   Sensors
       │        │        │
       └────────┼────────┘
                ▼
        Central Control
```

---

# 📈 Future System Architecture

A future version could evolve from a local embedded controller into a complete connected smart-kitchen platform:

```text
                Sensors
                   │
                   ▼
             ESP32 Controller
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
     Cooktop    Stirrer    Buzzer
      Relay      Motor
        │
        ▼
   Cooking Process
        │
        ▼
      Feedback
        │
        ▼
   Wi-Fi / IoT Layer
        │
   ┌────┴─────┐
   ▼          ▼
Mobile App  Cloud
```

---

# ⭐ Project Highlights

* 🔹 **ESP32-based smart cooktop**
* 🔹 Real-time overflow detection
* 🔹 HC-SR04 ultrasonic sensing
* 🔹 MLX90614 infrared temperature monitoring
* 🔹 Automatic cooktop shutdown
* 🔹 Relay-based safety control
* 🔹 Automated motor-driven stirring
* 🔹 L298N motor control
* 🔹 OLED real-time monitoring
* 🔹 Embedded C implementation
* 🔹 Threshold-based safety logic
* 🔹 Reported **~45% reduction in boil-over incidents**
* 🔹 Reported **~38% improvement in kitchen safety**
* 🔹 **23% faster automated response**
* 🔹 Temperature control within **±1.2°C**
* 🔹 Extensible toward IoT and AI-based cooking

---

# 📚 Project Report

The complete project documentation contains the detailed design methodology, experimental results, and system analysis.

**Smart Cooktop using ESP32 — Project Report**

The report can be included in the repository under:

```text
docs/
└── Smart_Cooktop_Project_Report.pdf
```

---

# 👩‍💻 Project Context

**Course:** Microprocessors and Microcontrollers
**Institution:** Vellore Institute of Technology, Chennai

The project demonstrates the practical application of:

```text
Microcontroller
      +
Sensors
      +
Actuators
      +
Control Logic
      +
User Interface
      =
Automated Embedded System
```

---

# 📌 Keywords

`ESP32` `Embedded Systems` `Embedded C` `Smart Cooktop` `Smart Kitchen` `IoT` `Kitchen Automation` `HC-SR04` `MLX90614` `Ultrasonic Sensor` `Temperature Sensor` `Relay` `L298N` `DC Motor` `Automated Stirring` `OLED` `SSD1306` `Sensor Integration` `Real-Time Monitoring` `Safety Automation` `Microcontrollers` `Embedded Control`

---

<p align="center">

**Sense → Decide → Control → Monitor → Automate**

</p>
