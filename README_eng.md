# Roller-Blind: Smart Automated Roller Blind on ESPHome

![ESPHome Logo](https://avatars.githubusercontent.com/u/36563322?s=200&v=4) ![Home Assistant Logo](https://avatars.githubusercontent.com/u/13844975?s=200&v=4)

## 🌟 Project Description

**Roller-Blind** is an intelligent roller blind control system based on ESPHome and integrated with Home Assistant. The project enables automatic control of the blind depending on the time of day, sun position, or user settings, with support for power-saving deep sleep mode. It is ideal for smart homes, providing comfort, safety, and energy efficiency.

Key features:
- **Automatic control**: The blind opens/closes on schedule (time of day) or according to the sun’s position (sunrise/sunset).
- **Manual control**: Via button, IR remote, or Home Assistant.
- **Power saving**: ESP32-C3 deep sleep with wake-up by timer or button.
- **Safety**: Motor overheat and timeout protection.
- **Monitoring**: OLED display shows status, battery, time, and wake-up reasons.
- **Integration**: Full compatibility with Home Assistant for remote control and monitoring.

The project is based on ESP32-C3, a stepper motor with A4988 driver, and various sensors. The code is written in ESPHome YAML using substitutions for flexible configuration.

---

## 🚀 Key Features

### Automation
- **“Sun” mode**: The blind follows sunrise and sunset with configurable sun angle offset.
- **“Time” mode**: User-defined open and close times (e.g., 07:00–18:00).
- **Auto/Manual mode**: Enable or disable automatic control.
- **Deep sleep**: The ESP32 sleeps until the next event, saving battery.

### Control
- **Physical button**: Multifunctional (open/close, stop, sleep, info, learning).
- **IR remote (LG)**: Commands UP, DOWN, STOP.
- **Home Assistant**: Full control via MQTT/API (open, close, position).
- **Learning**: Calibration of blind endpoints (open/closed).

### Safety and Monitoring
- **Fuse**: Current monitoring (INA226) and timeouts to prevent overheating.
- **Reed switch**: Position sensor (for stopping when closed).
- **RTC DS1307**: Accurate time even without Wi-Fi.
- **OLED display**: Status info, battery level, sleep timer, and wake-up reasons.
- **Boot log**: History of wake-up causes (timer, button, etc.).

### Power Saving
- Battery powered (18650) with voltage monitoring.
- Deep sleep with wake-up by timer or GPIO.
- Wi-Fi enabled only when needed.

---

## 🛠 Components and Hardware

### Main Components
- **Microcontroller**: ESP32-C3 (low power, Wi-Fi/BLE).
- **Stepper motor**: 28BYJ-48 with A4988 driver (direction, step, sleep control).
- **Display**: OLED SSD1306 I2C (128x64 for status display).
- **Sensors**:
  - INA226: Motor current and voltage monitoring.
  - ADC: Battery voltage measurement.
  - DS1307: RTC for accurate timekeeping.
  - Reed switch: Blind position detection.
- **Power**: 18650 battery (3.7V) with DC-DC converter.
- **Optional**: IR receiver, button, IR remote.

### Wiring Diagram (ESP32-C3 GPIO)
- GPIO0: Control button (pull-up, inverted).
- GPIO1: ADC for battery voltage.
- GPIO3: Step pin for A4988.
- GPIO4: Direction pin for A4988 (inverted).
- GPIO2: Sleep pin for A4988.
- GPIO5: Motor power (GPIO switch).
- GPIO6: Reed switch (pull-up, inverted).
- GPIO7: Display power (GPIO switch).
- GPIO8: LED (optional).
- GPIO9: SDA I2C.
- GPIO10: SCL I2C.

### Software Dependencies
- **ESPHome**: Version 2024+ (supports deep sleep and sensors).
- **Home Assistant**: For integration (MQTT, API).
- **Font**: Arial.ttf (for display, uploaded to ESPHome).

---

## 📋 Installation and Setup

### 1. Prepare Hardware
- Assemble the circuit according to the GPIO wiring above using the schematic.
- Install ESPHome firmware on ESP32-C3 (via USB or OTA).
- Connect the battery and test power supply.

### 2. Clone Repository
```bash
git clone https://github.com/your-username/roller-blind.git
cd roller-blind
```

### 3. Configure ESPHome
- Open `roller-blind.yaml` in VS Code with ESPHome extension.
- Edit substitutions at the top of the file to match your setup:
  ```yaml
  substitutions:
    name: roller-blind2
    friendly_name: roller-blind2
    friendly_name_short: roller_blind2
    version: "04.10.2025"
    device_ip: 192.168.x.x  # Your IP
    # ... other parameters (GPIO, speeds, etc.)
  ```
- Upload the `arial.ttf` font file to the project folder (or specify path).

### 4. Integrate with Home Assistant
- Add device via MQTT discovery in Home Assistant.
- Use entities: cover, sensors, switches, buttons.
- Example automation: “Open blind at sunrise.”

### 5. Blind Learning Mode
- Enable learning mode by long-pressing the button for 6–10 seconds.
- Follow on-screen instructions: close the blind, press button at bottom position, then open and press at top position.

---

## 🎮 Usage

### Button Control (GPIO0)
- **1 press**: Open/Close (toggle direction).
- **2 presses**: Sleep mode on/off.
- **3 presses**: Info screen.
- **4 presses**: Learning screen.
- **5 presses**: Wi-Fi on/off.
- **6 presses**: Switch display pages.
- **Long press (3–5 sec) on error screen**: Restart.
- **Long press (6–10 sec)**: Enter learning mode.

### IR Remote Control
- **xxx**: Train via logs.

### Display Pages
- **main_page**: Main status (time, position, battery, sleep).
- **learning**: Learning instructions.
- **info**: Button help.
- **start_learning/open_setting/close_setting**: Learning steps.
- **finish**: Learning success.
- **sleeping**: Sleep screen.
- **safety_alarm**: Error (overheat/timeout).

### Home Assistant Settings
- **Sunrise/Sunset Offset**: Adjust sun angle offset.
- **Brightness**: Display brightness.
- **Stepper Speed**: Motor speed.
- **Max Current/Timeout**: Safety settings.
- **Blind Position**: Manual position control.

---

## 🔧 Advanced Configuration

### Substitutions
Configure parameters at the top of YAML:
- `pin_a/pin_b/pin_c`: GPIOs for A4988.
- `max_speed/run_duration`: Motor speed and run time.
- `device_ip`: Device IP address.

### Global Variables
- `endstop`: Endpoint calibration (set during learning).
- `wifi_enabled`: Wi-Fi control.
- `sun_time`: Sun/time mode toggle.

### Logs and Debugging
- Enable `logger: debug` for detailed logs.
- Monitor via ESPHome logs or Home Assistant.

### Power Saving
- Deep sleep lasts until next event (sunrise/sunset or user time).

---

## 📊 Screenshots and Videos

*(Add display screenshots, HA dashboard images, or demo videos here)*

- [Display in action](screenshot1.png)
- [Home Assistant integration](screenshot2.png)

---

## 🐛 Troubleshooting

- **Blind doesn’t move**: Check motor power (GPIO5) and calibration.
- **No time**: Sync RTC via HA or Wi-Fi.
- **Error triggered**: Check current (INA226) and timeouts.
- **Wi-Fi won’t connect**: Verify `wifi.yaml` settings.

For help, open an issue in the repository.

---

## 📄 License

This project is licensed under the MIT License. Use at your own risk.

---

## 🙏 Acknowledgments

- ESPHome community for the excellent framework.
- Home Assistant for integration.
- Thank you for using! If the project is useful, please ⭐ on GitHub.

---

*Made with ❤️ by ParusSmartHome. Version: 04.10.2025*
