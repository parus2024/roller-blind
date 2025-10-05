# Smart-Roller-Blind: Smart Automated Standalone Roller Blind on ESPHome

![ESPHome Logo](roller-blind-img/esphome1.png) ![Home Assistant Logo](https://avatars.githubusercontent.com/u/13844975?s=200&v=4)
<p align="center">
    <img alt="Static Badge" src="https://img.shields.io/badge/made%20by-ParusSmartHome-blue">
    <img alt="Static Badge" src="https://img.shields.io/badge/version-v2.0%20Beta-green">
    <img alt="Static Badge" src="https://img.shields.io/badge/esphome min version-2025.7.5-red">
    <img alt="Static Badge" src="https://img.shields.io/badge/license-MIT-orange">
</p>

## 🌟 Project Description

**Smart-Roller-Blind** is an intelligent roller blind (curtain) control system built on ESPHome and integrated with Home Assistant. The project allows automatic and manual control of the blind depending on the time of day, sun position, or user settings, with support for energy-saving deep sleep mode. Perfect for smart homes, providing comfort, safety, and energy efficiency. Without recharging, it provides autonomous operation for 1-2 months with daily use. By replacing the stepper motor with a driver with more powerful options, it can work with heavy curtains.

Main features:
- **Automatic control**: The blind is controlled by schedule (time input) or by sun position (sunrise/sunset), always maintaining the desired state.
- **Manual control**: Via button, IR remote, Home Assistant, Yandex (Alice), Web page.
- **Energy saving**: Deep sleep of ESP32-C3 with wake-up by timer or button.
- **Safety**: Motor overheat protection and timeouts.
- **Monitoring**: OLED display shows status, battery, time, and wake-up reasons.
- **Integration**: Full compatibility with Home Assistant for remote control and monitoring.

The project is based on ESP32-C3 (or any ESP32), stepper motor with A4988 driver, and various sensors. The code is written in ESPHome YAML using substitutions for flexible configuration.

---

## 🚀 Key Features

### Automation
- **"Sun" mode**: The blind works according to sunrise and sunset (with adjustable sun angle offset).
- **"Time" mode**: Set opening and closing times (e.g., 07:00–18:00).
- **Auto/Manual mode**: Enable/disable automatic control. In manual mode, controls the blind only on timer wake-up.
- **Deep sleep**: ESP32 sleeps until the next event, saving battery.
- **Auto-correction**: Ability to correct blind position by reed switch on opening.

### Control
- **Physical button**: Multifunctional (open/close, stop, sleep, help, learning).
- **IR remote**: Programmable for any code on any IR remote.
- **Home Assistant**: Full control via API (open, close, position).
- **Yandex (Alice)**: When connected via Home Assistant (open, close, position).
- **Web page**: Remote control from anywhere via any browser at the device's IP address.
- **Learning**: Calibration of blind end points (open/closed).

### Safety and Monitoring
- **Protector**: Current monitoring (INA226) and timeouts to prevent overheating.
- **Reed switch**: Position sensor (for stopping on closing).
- **RTC DS1307**: Accurate time even without Wi-Fi.
- **OLED display**: Information on status, battery, time to sleep, and wake-up reasons.
- **Boot log**: History of wake-up reasons (timer, button, etc.).
- **ESP info**: Information about ESP (reboot reason, free memory, etc.).

### Energy Saving
- Battery power (18650) with voltage monitoring.
- Deep Sleep with wake-up by timer or GPIO.
- Wi-Fi turns on only when necessary.

---

## 🛠 Components and Hardware

### Main Components
- **Microcontroller**: ESP32-C3 (low power consumption, Wi-Fi/BLE).
- **Stepper motor**: 28BYJ-48 5V in bipolar connection mode.
- **Charge controller**: TP4056 charge controller with protection.
- **Motor driver**: A4988 driver (direction, step, sleep control).
- **Display**: OLED SSD1306 I2C (128x64, for status display).
- **Sensors**:
  - INA226 (INA219): Motor current and voltage monitoring.
  - ADC: Battery voltage.
  - DS1307: RTC for autonomous time. Corrected by time from Home Assistant.
- **Power**: 18650 battery (3.7V) with DC-DC converter and USB-C charging capability.
- **DC-DC 3.7->8-12V**: Boost voltage regulator.
- **DC-DC 3.3V**: Buck linear voltage regulator HT7333.
- **MOSFETs**: MOSFETs with appropriate circuitry.
- **Solar panel**: 5.5-6V solar panel with 10mA charging current.
- **Additional**: IR receiver, button, IR remote (optional).

### Wiring Diagram (ESP32-C3 GPIO)
- GPIO0: Control button (with pull-up, inverted).
- GPIO1: ADC for battery.
- GPIO3: Step (step) for A4988.
- GPIO4: Direction (dir) for A4988 (inverted).
- GPIO2: Sleep (sleep) for A4988.
- GPIO5: Motor power (GPIO switch).
- GPIO6: Reed switch (pull-up, inverted).
- GPIO7: Display power (GPIO switch).
- GPIO8: LED (optional).
- GPIO9: SDA I2C.
- GPIO10: SCL I2C.

### Software Dependencies
- **ESPHome**: Version 2024+ (deep sleep and sensor support).
- **Home Assistant**: For integration (MQTT, API).
- **Font**: Arial.ttf (for display, loaded in ESPHome).

---

## 📋 Installation and Setup

### 1. Hardware Preparation
- Assemble the circuit according to the GPIO above, using the electrical schematic.
- Install ESPHome on ESP32-C3 (via USB or OTA).
- Connect the battery and test the power.

### 2. Repository Cloning
```bash
git clone https://github.com/your-username/roller-blind.git
cd roller-blind
```

### 3. ESPHome Configuration
- Open the `roller-blind.yaml` file in VS Code with ESPHome extension or in ESPHome Builder.
- Change substitutions at the beginning of the file to your configuration:
  ```yaml
  substitutions:
    name: smart-roller-blind
    friendly_name: Smart Roller Blind
    friendly_name_short: smart_roller_blind
    version: "05.10.2025"
    device_ip: 192.168.x.x  # Your IP
    # ... other parameters (GPIO, speeds, etc.)
  ```
- Load the necessary include folders (and specify the path) or create your own basic entries for the ESPHome project.
- Load the font `arial.ttf` into the project folder (and specify the path).

### 4. Home Assistant Integration
- Add the device to HA via MQTT discovery.
- Use entities: cover, sensors, switches, buttons.
- Example automation: "Open the blind at dawn".

### 5. Blind Learning
- Enable learning mode (long press button 6-10 sec).
- Follow display instructions: close the blind, press button at bottom point, then open and press at top.

---

## 🎮 Usage

### Control via Button (GPIO0)
- **1 press**: Open/Close/Stop (direction toggle).
- **2 presses**: Enable/Disable sleep mode.
- **3 presses**: Help (info screen).
- **4 presses**: Learning screen.
- **5 presses**: Enable/Disable Wi-Fi.
- **6 presses**: Switch display pages.
- **Long press (3-5 sec) on main screen**: Toggle auto/manual mode.
- **Long press (3-5 sec) on alarm screen**: Reboot.
- **Long press (6-10 sec)**: Enter learning mode.

### Control via IR Remote
- **xxx**: Learn from logs.
- **Disable button**: Disable IR sensor if not used, saving up to 50% in sleep mode.

### Display (Pages)
- **main_page**: Main status (time, position, battery, sleep).
- **learning**: Learning instructions.
- **info**: Button help.
- **start_learning/open_setting/close_setting**: Learning steps.
- **finish**: Learning success.
- **sleeping**: Sleep screen.
- **safety_alarm**: Alarm (overheat/timeout).

### Settings in Home Assistant
- **Sunrise/Sunset Offset**: Sun angle correction.
- **Brightness**: Display brightness.
- **Stepper Speed**: Motor speed.
- **Max Current/Timeout**: Safety.
- **COVER**: Manual position setting.
- **Other settings**: Numerous other settings.

---

## 🔧 Advanced Configuration

### Substitutions
Configure parameters at the beginning of YAML:
- `pin_a/pin_b/pin_c`: GPIO for A4988.
- `max_speed/run_duration`: Speed and runtime.
- `device_ip`: Device IP address.

### Global Variables
- `endstop`: End point (calibrated during learning).
- `wifi_enabled`: Wi-Fi control.
- `sun_time`: Sun/time mode.
- Other variables.

### Logs and Debugging
- Enable `logger: debug` for logs.
- Monitor via ESPHome logs or HA.

### Energy Saving
- Deep Sleep lasts until the next event (sunrise/sunset or custom time). Without recharging (solar panel or USB-C), operates up to 2 months.

---

## 📊 Screenshots and Videos

*(Add screenshots of the display, HA dashboard, or demo videos here)*

- [Firmware yaml file](smart-roller-blind.yaml)
- [Appearance](roller-blind-img/roller-blind-v2_1.jpg)
- [Appearance](roller-blind-img/roller-blind-v2_1.jpg)
- [Window view](roller-blind-img/window.jpg)
- [Wiring diagram](roller-blind-img/smart-roller-blind-shema.jpg)
- [Video#1 YOUTUBE](https://youtu.be/TDQovWRaWWA)
- [Video#2 YOUTUBE](https://youtu.be/KZsbmizrDjA)
- [3D print files](roller-blind-3d)

---

## 🐛 Possible Issues

- **Blind doesn't move**: Check motor power (GPIO5) and calibration.
- **No time**: Sync RTC via HA or Wi-Fi.
- **Alarm**: Check current (INA226) and timeouts.
- **Wi-Fi doesn't connect**: Check `wifi.yaml` settings.

For help, open an issue in the repository.

---

## 📄 License

This project is distributed under the MIT license. Use at your own risk.

---

## Additional Information Sources

Telegram channel https://t.me/parus_smart

---
## 🙏 Acknowledgments

- ESPHome community for the great framework.
- Home Assistant for integration.
- You for using it! If the project is helpful, give it a ⭐ on GitHub.

---

*Created with ❤️ ParusSmartHome. Version: 05.10.2025*
