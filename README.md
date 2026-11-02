# IoT EV Charging Station

<p align="center">
  <a href="https://github.com/manojhmanu6-hub/IoT-EV-Charging-Station">
    <img src="https://img.shields.io/badge/GitHub-Repository-black?style=for-the-badge&logo=github" alt="GitHub Repository">
  </a>
  <img src="https://img.shields.io/badge/Platform-ESP32-blue?style=for-the-badge&logo=espressif" alt="ESP32">
  <img src="https://img.shields.io/badge/Domain-Smart%20Energy-green?style=for-the-badge" alt="Smart Energy">
  <img src="https://img.shields.io/badge/Language-C%2B%2B-orange?style=for-the-badge&logo=cplusplus" alt="C++">
</p>

## 📌 Project Overview

The **IoT EV Charging Station** is an intelligent, automated electric vehicle charging management system designed to monitor power delivery, handle user authentication, track energy consumption, and provide remote status monitoring.

The system uses an ESP32 microcontroller connected to current/voltage sensors, an RFID reader for user verification, an LCD display, a relay module to control power flow, and cloud/Wi-Fi connectivity to log charging metrics in real time.

When an authorized user scans their RFID tag, the system unlocks power delivery, monitors the charging cycle, calculates energy consumption, and safely shuts off when complete or disconnected.

## 🎯 Objectives

- Authenticate users securely using RFID technology.
- Control high-voltage charging relays safely via microcontrollers.
- Monitor real-time voltage, current, and power parameters.
- Display charging status, cost, and energy metrics on an LCD panel.
- Transmit charging session data over Wi-Fi to a cloud dashboard.
- Provide manual override and emergency cut-off mechanisms.

## ✨ Key Features

1. **RFID User Authentication:** Restricts charging access to authorized cardholders.
2. **Real-Time Power Monitoring:** Continuously measures voltage and current consumption during a session.
3. **Automated Relay Control:** Safely connects or cuts off power delivery to the EV socket.
4. **LCD Status Display:** Shows live session status, power consumption, and total cost/time.
5. **Wireless Cloud Logging:** Sends telemetry data over Wi-Fi for remote monitoring and billing tracking.
6. **Overload & Fault Protection:** Automatically terminates charging if abnormal electrical parameters are detected.
7. **Manual Stop Button:** Allows users to manually end the charging session at any time.

## 🛠️ Hardware Components

| Component | Description |
|---|---|
| ESP32 DevKit V1 | Main microcontroller with Wi-Fi |
| RC522 RFID Reader | User authentication module |
| PZEM-004T / Current Sensor | Voltage and current measurement module |
| Relay Module (5V/12V) | High-power switching relay for EV supply |
| 16×2 I2C LCD | Session information display |
| Push Button | Manual stop / start control |
| Status LEDs (RGB/Single) | Visual state indicators (Idle, Charging, Error) |
| Jumper Wires & Power Supply | Circuit wiring and regulated power |

## 💻 Software Requirements

- Arduino IDE
- ESP32 Board Package
- Embedded C/C++
- Wire Library
- LiquidCrystal_I2C Library
- MFRC522 Library
- WiFi Library
- HTTPClient / MQTT Library

## 🔌 Pin Configuration

| Component | ESP32 GPIO |
|---|---|
| RFID SS (SDA) | GPIO 5 |
| RFID RST | GPIO 27 |
| RFID MOSI | GPIO 23 |
| RFID MISO | GPIO 19 |
| RFID SCK | GPIO 18 |
| Relay Control | GPIO 26 |
| Current Sensor (Analog) | GPIO 34 |
| I2C SDA | GPIO 21 |
| I2C SCL | GPIO 22 |
| Push Button | GPIO 14 |

**Note:** The LCD operates over the I2C bus with address `0x27`.

## ⚙️ Working Principle

### 1. System Initialization
The ESP32 initializes all peripherals, connects to the Wi-Fi network, sets up the RFID reader, and displays a "System Ready / Tap Card" message on the LCD.

### 2. User Authentication
A user taps their RFID card on the RC522 reader. The ESP32 verifies the UID against stored authorized tags.

### 3. Charging Initiation
If authentication succeeds:
- The relay triggers to close the circuit, supplying power to the EV.
- The LCD updates to show the **Charging Active** status along with live metrics.

### 4. Live Session Monitoring
Sensors continuously measure voltage, current, and energy consumption. If an overload occurs or the user presses the stop button, the session terminates immediately.

### 5. Session Completion & Data Logging
Once charging finishes or the user taps the card again to stop:
- The relay cuts off power.
- Total energy and duration are calculated and sent via Wi-Fi to the cloud database.

## 🔄 System Workflow

```text
          SYSTEM START
                |
                v
        Initialize ESP32
                |
                v
        Connect to Wi-Fi
                |
                v
        Display "Ready / Tap Card"
                |
           RFID Card Tapped?
              /       \
            No         Yes
            |           |
         Wait      Card Authorized?
                      /       \
                    No         Yes
                    |           |
               Access Denied   Unlock Relay
                                |
                                v
                        Start Charging Loop
                                |
                                v
                         Monitor Voltage/Current
                                |
                        Session Ended / Stop?
                              /       \
                            No         Yes
                            |           |
                         Continue     Cut Off Relay
                                        |
                                        v
                               Send Cloud Telemetry
                                        |
                                        v
                                Reset to Ready State

