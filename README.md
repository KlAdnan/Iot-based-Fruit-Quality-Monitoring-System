# IoT-Based Fruit Quality Monitoring System

![System Overview](images/real%20overview.png)
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![Arduino](https://img.shields.io/badge/Arduino-Uno-teal.svg)](https://www.arduino.cc/)
[![IoT](https://img.shields.io/badge/IoT-ThingSpeak-blue.svg)](https://thingspeak.com/)

An IoT-based fruit quality monitoring system using Arduino Uno, gas sensors (MQ-3/MQ-4), temperature and humidity sensors (DHT11), ESP8266 Wi-Fi module, and ThingSpeak platform for real-time data visualization and quality assessment.

![Pin Diagram](images/pin%20diagram.jpeg)

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Hardware Components](#hardware-components)
- [Software Requirements](#software-requirements)
- [System Architecture](#system-architecture)
- [Installation](#installation)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Circuit Diagram](#circuit-diagram)
- [ThingSpeak Integration](#thingspeak-integration)
- [Results](#results)
- [Advantages](#advantages)
- [Future Scope](#future-scope)
- [Acknowledgments](#acknowledgments)
- [License](#license)

## 🎯 Overview

Food safety and hygiene are critical concerns in preventing food wastage. This project addresses the challenge of monitoring atmospheric factors that affect food quality during storage and transportation. The system uses various sensors to monitor temperature, humidity, methane, and alcohol levels, providing real-time quality assessment.

**Key Innovations:**
- Real-time environmental monitoring
- IoT-enabled remote access via ThingSpeak
- Automatic quality assessment
- LCD display for local monitoring
- Cost-effective solution for food storage facilities

## ✨ Features

- **Real-Time Monitoring**: Continuous tracking of environmental parameters at 25 FPS
- **Multi-Sensor System**:
  - MQ-3: Alcohol/ethanol gas detection
  - MQ-4: Methane gas detection
  - DHT11: Temperature and humidity measurement
- **IoT Integration**: Wireless data transmission to ThingSpeak cloud platform
- **Quality Assessment**: Automatic determination of fruit quality (Good/Bad)
- **Local Display**: 16x2 LCD with I2C interface for on-site monitoring
- **Remote Access**: View data from anywhere via web interface

## 🔧 Hardware Components

| Component | Specification | Quantity | Purpose |
|-----------|---------------|----------|----------|
| Arduino Uno | ATmega328P, 16 MHz | 1 | Main microcontroller |
| DHT11 Sensor | Temperature: 0-50°C, Humidity: 20-80% | 1 | Environmental monitoring |
| MQ-3 Sensor | Alcohol detection: 0.04-4 mg/L | 1 | Ethanol gas sensing |
| MQ-4 Sensor | Methane detection: 300-10000 ppm | 1 | Methane gas sensing |
| ESP8266 Wi-Fi | 802.11 b/g/n, 10m range | 1 | Wireless connectivity |
| LCD 16x2 with I2C | I2C interface | 1 | Local data display |
| Power Supply | 5V DC | 1 | System power |

## 💻 Software Requirements

- **Arduino IDE** (version 1.8+)
- **Libraries**:
  - DHT.h (DHT sensor library)
  - Wire.h (I2C communication)
  - LiquidCrystal_I2C.h (LCD display)
  - ESP8266WiFi.h (Wi-Fi connectivity)
  - ThingSpeak.h (IoT platform integration)
  - SPI.h (SPI communication)

## 🏛️ System Architecture

```
[Sensors] --> [Arduino Uno] --> [LCD Display]
    |              |                   |
    |              v                   |
    |      [ESP8266 Wi-Fi]             |
    |              |                   |
    |              v                   |
    |      [ThingSpeak Cloud]          |
    |              |                   |
    +-------> [Quality Assessment] <---+
                   |
                   v
            [Good/Bad Display]
```

![System Flowchart](images/flowchart.png)

## 💾 Installation

### Hardware Setup

1. **Connect DHT11 Sensor**:
   - VCC → 5V
   - GND → GND
   - Data → Digital Pin 2

2. **Connect MQ-3 Sensor**:
   - VCC → 5V
   - GND → GND
   - Analog Out → A0

3. **Connect MQ-4 Sensor**:
   - VCC → 5V
   - GND → GND
   - Analog Out → A1

4. **Connect ESP8266**:
   - VCC → 3.3V (Important: Do NOT use 5V)
   - GND → GND
   - TX → RX (Arduino)
   - RX → TX (Arduino)

5. **Connect LCD with I2C**:
   - VCC → 5V
   - GND → GND
   - SDA → A4
   - SCL → A5

### Software Setup

1. Install Arduino IDE
2. Install required libraries via Library Manager
3. Configure Wi-Fi credentials in code:
   ```cpp
   const char* ssid = "your_wifi_ssid";
   const char* password = "your_wifi_password";
   ```
4. Set up ThingSpeak channel and API key
5. Upload code to Arduino Uno

## 🚀 Usage

1. Power on the system
2. Wait for Wi-Fi connection (indicated on LCD)
3. Place fruits in the monitored environment
4. System will:
   - Read sensor data every 10 seconds
   - Display readings on LCD
   - Upload data to ThingSpeak
   - Assess fruit quality
   - Display "Good Fruit" or "Bad Fruit" based on thresholds

## 🔍 How It Works

### Sensor Data Collection

1. **Temperature & Humidity (DHT11)**:
   - Optimal range: 20-35°C, 40-100% humidity
   - Readings taken every 10 seconds

2. **Alcohol Detection (MQ-3)**:
   - Detects ethanol from fruit fermentation
   - Threshold: < 500 (analog value)

3. **Methane Detection (MQ-4)**:
   - Detects methane from decay
   - Threshold: < 400 (analog value)

### Quality Assessment Algorithm

```cpp
bool isGoodFruit = (temperature >= 20 && temperature <= 35 &&
                    humidity >= 40 && humidity <= 100 &&
                    mq3Value < 500 &&
                    mq4Value < 400);
```

If all conditions are met: **Good Fruit**  
If any condition fails: **Bad Fruit**

### Data Flow

1. Arduino reads all sensors
2. Data displayed on LCD:
   - Temperature, Humidity
   - Alcohol level, Methane level
   - Quality status
3. ESP8266 transmits data to ThingSpeak
4. ThingSpeak updates fields:
   - Field 1: Temperature
   - Field 2: Humidity
   - Field 3: Alcohol (MQ-3)
   - Field 4: Methane (MQ-4)

![circuit diagram IOT fruit Quality monitoring System](https://github.com/user-attachments/assets/e1c3ceb8-16d5-4307-8157-65bc6814618a)

## 📊 ThingSpeak Integration

ThingSpeak provides cloud-based data storage and visualization:

1. Create ThingSpeak account
2. Create new channel with 4 fields
3. Obtain Write API Key
4. Configure in Arduino code:
   ```cpp
   const unsigned long channelNumber = YOUR_CHANNEL_NUMBER;
   const char* apiKey = "YOUR_API_KEY";
   ```
5. Data updates every 10 seconds
6. View real-time graphs on ThingSpeak dashboard

## 🏆 Results

The system successfully:
- Monitors environmental parameters in real-time
- Accurately detects fruit quality degradation
- Provides local and remote access to data
- Displays quality status on LCD
- Uploads data to ThingSpeak for historical analysis
  
---


**Performance Metrics:**
- Sensor reading interval: 10 seconds
- Data update rate: Real-time
- Temperature accuracy: ±2°C
- Humidity accuracy: ±5%
- Wi-Fi range: Up to 10 meters

## ✅ Advantages

1. **Real-time Monitoring**: Continuous tracking prevents spoilage
2. **Remote Access**: Monitor from anywhere via internet
3. **Predictive Analytics**: Historical data helps predict quality degradation
4. **Quality Assurance**: Maintains optimal storage conditions
5. **Cost-Effective**: Affordable components and open-source software
6. **Automation**: Reduces manual monitoring effort
7. **Traceability**: Complete record of environmental conditions

## 🔮 Future Scope

- Integration with GSM module for SMS alerts
- Machine learning for improved quality prediction
- Mobile app for enhanced user experience
- Multi-chamber monitoring support
- Integration with refrigeration control systems
- Support for different fruit types with custom thresholds
- Miniaturization for portable use
- Battery backup for continuous operation


![Bad Fruit Detection Demo](images/bad%20fruit%20demo.jpeg)
## 🙏 Acknowledgments

We express our sincere gratitude to:
- College of Engineering Thalassery for providing facilities
- Dr. Sudheer VR, Head of Department of ECE
- Ms. Safoora OK, Project Supervisor
- Mr. Jinesh S, Project Co-ordinator

This project was completed as part of the B.Tech degree requirements in Electronics and Communication Engineering under APJ Abdul Kalam Technological University during the academic year 2023-2024.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.



**Note**: This is an academic project designed for educational purposes. Clinical or commercial implementation would require additional safety certifications and regulatory approvals.
