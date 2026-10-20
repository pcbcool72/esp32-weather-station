# ESP32 Weather Station Using DHT11/DHT22 and OLED

![ESP32 Weather Station Using DHT11/DHT22 and OLED](https://pcbcool.com/wp-content/uploads/2025/12/Step-by-Step-Guide-to-DIY-ESP32-Weather-Station.jpg.webp)

A DIY ESP32-based weather station that measures temperature and humidity using a DHT11 or DHT22 sensor. The readings can be monitored through the Arduino IDE Serial Monitor or displayed on an optional SSD1306 OLED screen.

This project is suitable for electronics enthusiasts, students, and embedded systems developers who want to explore environmental sensing, microcontroller interfacing, and the fundamentals of IoT development.

## Overview

An ESP32 weather station is a compact environmental monitoring system that collects measurements from sensors, processes the readings using a microcontroller, and presents the information locally or through a network.

In this project, the ESP32 acts as the central controller. A DHT11 or DHT22 sensor measures temperature and relative humidity, while an optional OLED display provides a convenient way to view the latest measurements without connecting to a computer.

The ESP32 is well suited to this application because it combines processing capabilities with built-in Wi-Fi and Bluetooth connectivity. This makes it possible to extend the basic monitoring system with wireless data transmission, cloud integration, and additional environmental sensors.

The project starts with a simple wired setup and can later be expanded into a more comprehensive IoT weather monitoring system.

## Features

- Measures ambient temperature in degrees Celsius.
- Measures relative humidity as a percentage.
- Supports DHT11 and DHT22/AM2302 sensors.
- Displays readings through the Arduino IDE Serial Monitor.
- Supports an optional 128 × 64 SSD1306 OLED display.
- Uses the I²C interface for OLED communication.
- Provides a foundation for future Wi-Fi and cloud-based monitoring.
- Uses commonly available components and a breadboard-friendly design.

## Hardware Requirements

| Component | Purpose |
|---|---|
| ESP32 development board | Reads sensor data and manages the application |
| DHT11 or DHT22 sensor | Measures temperature and humidity |
| SSD1306 OLED display (optional) | Displays live environmental readings |
| 10 kΩ resistor, if required | Pull-up resistor for a bare four-pin DHT sensor |
| Breadboard | Allows temporary circuit assembly |
| Jumper wires | Connect the ESP32, sensor, and display |
| USB cable | Powers the ESP32 and uploads firmware |
| 5 V USB power adapter (optional) | Provides power for standalone operation |

### Choosing the DHT Sensor

**DHT11:** An economical option for basic environmental monitoring and beginner projects.

**DHT22:** Offers improved measurement accuracy and a wider operating range than the DHT11, making it suitable for more demanding applications.

Both sensors can be used with the same general project architecture. The selected sensor type must match the configuration in the firmware.

## System Architecture

The system consists of three main functional blocks:

1. **Sensing:** The DHT sensor measures temperature and relative humidity.
2. **Processing:** The ESP32 reads and validates the sensor measurements.
3. **Output:** The measurements are sent to the Serial Monitor and, when installed, the OLED display.

The basic data flow is:

`DHT11/DHT22 → ESP32 → Serial Monitor / OLED Display`

The OLED communicates with the ESP32 over I²C. The basic version does not require an internet connection. Wi-Fi-based remote monitoring can be introduced as a future enhancement.

## Circuit Connections

### DHT11/DHT22 Sensor to ESP32

| DHT Sensor Pin | ESP32 Connection | Function |
|---|---|---|
| VCC (+) | 3V3 | Sensor power |
| GND (-) | GND | Common ground |
| DATA / OUT | GPIO 4 | Digital sensor data |

### Optional SSD1306 OLED to ESP32

| OLED Pin | ESP32 Connection | Function |
|---|---|---|
| VCC | 3V3 | Display power |
| GND | GND | Common ground |
| SDA | GPIO 21 | I²C data |
| SCL | GPIO 22 | I²C clock |

These are the pin assignments used by this project. Other ESP32 boards may require adjustments depending on their pin mapping and peripheral configuration.

### Important Wiring Notes

- Verify the sensor's voltage requirements before connecting it to the ESP32.
- Connect all grounds together, including the grounds of any separately powered components.
- A bare four-pin DHT sensor typically requires a pull-up resistor, commonly 10 kΩ, between VCC and DATA.
- Many three-pin DHT sensor modules already include a pull-up resistor.
- Check the OLED module's supply-voltage requirements and I²C address. The example firmware uses the SSD1306 address `0x3C`.
- Keep wiring short and organized to reduce connection problems and simplify troubleshooting.

## How It Works

The firmware follows a repeating measurement cycle.

1. **Initialization:** The ESP32 initializes serial communication, the DHT sensor, and the OLED display if present.
2. **Measurement interval:** The application waits approximately two seconds between readings.
3. **Sensor acquisition:** Temperature and relative humidity are read from the DHT sensor.
4. **Data validation:** Invalid readings are detected before they are used.
5. **Serial output:** Valid readings are printed to the Serial Monitor.
6. **Display update:** The OLED screen is refreshed with the latest temperature and humidity values.
7. **Continuous monitoring:** The process repeats while the ESP32 remains powered.

If the sensor returns invalid data, the firmware reports the error through serial output and skips the current measurement cycle.

## Software Requirements

The project uses the Arduino IDE and the ESP32 board support package.

### Required Libraries

Install the following libraries through the Arduino IDE Library Manager:

- **DHT sensor library by Adafruit** — reads temperature and humidity from the DHT sensor.
- **Adafruit SSD1306** — controls the compatible OLED display.
- **Adafruit GFX Library** — provides graphics and text-rendering functions used by the OLED library.

The SSD1306 library is needed only when using the optional OLED display.

### Arduino IDE Setup

1. Install the Arduino IDE.
2. Install the ESP32 board support package using the Board Manager.
3. Select the appropriate ESP32 development board.
4. Connect the ESP32 to the computer using USB.
5. Install the required libraries through **Sketch → Include Library → Manage Libraries**.
6. Select the appropriate serial port.
7. Configure the sensor type and display settings in the firmware.
8. Upload the program and open the Serial Monitor at the baud rate configured by the firmware.

The Serial Monitor can be used to confirm that the ESP32 starts correctly, communicates with the sensor, and produces valid measurements.

## Project Assembly

### Step 1: Prepare the Breadboard

Place the ESP32 development board on the breadboard and identify its 3V3, GND, and GPIO pins.

Keep the sensor and optional OLED close to the ESP32 to minimize wiring complexity.

### Step 2: Connect the DHT Sensor

Connect the sensor's power, ground, and data pins according to the circuit connection table.

Use GPIO 4 for the data signal. If the sensor is a bare four-pin device, add the required pull-up resistor between its VCC and DATA pins.

### Step 3: Connect the OLED Display

If using an SSD1306 OLED, connect its SDA and SCL pins to GPIO 21 and GPIO 22, respectively.

Connect the display's VCC and GND to the appropriate ESP32 supply and ground connections.

### Step 4: Check the Circuit

Before powering the system, verify the pin assignments, supply voltage, polarity, and common ground connections.

Check that the sensor and display connections match the selected hardware modules.

### Step 5: Upload and Test the Firmware

Configure the Arduino IDE for the selected ESP32 board and sensor type.

Upload the firmware and inspect the Serial Monitor for temperature and humidity readings. If an OLED is installed, confirm that the display initializes and refreshes with the latest measurements.

## Understanding the Firmware

The firmware is organized around sensor initialization, repeated measurements, validation, and output.

### Sensor Initialization

The DHT library is initialized using the selected GPIO pin and sensor model. The firmware must distinguish between DHT11 and DHT22 because the two sensors use different measurement characteristics.

### OLED Initialization

The SSD1306 display is configured with a resolution of 128 × 64 pixels and the I²C interface. The example configuration uses address `0x3C`.

If the display cannot initialize, the application reports an error through the Serial Monitor.

### Periodic Measurements

The program reads temperature and humidity approximately every two seconds. This interval is appropriate for the example and is consistent with the typical sampling needs of DHT sensors.

### Data Validation

The firmware checks whether the returned values are valid. In the Arduino environment, `isnan()` can be used to identify invalid floating-point readings.

Invalid measurements should not be treated as real environmental values.

### Output and Display Updates

Valid readings are printed to the Serial Monitor. When the OLED is enabled, the display is cleared and refreshed with the latest temperature and humidity measurements.

This separation between sensor acquisition and output makes the application easier to extend with additional interfaces or monitoring features.

## Common Mistakes and Troubleshooting

### 1. Incorrect Supply Voltage

The ESP32 uses 3.3 V logic. Check the sensor and display module specifications before selecting their supply voltage. Do not assume that every module can safely operate at 5 V or that every module's signal outputs are compatible with 3.3 V logic.

### 2. Missing Common Ground

The ESP32 and connected peripherals need a shared electrical reference. If a sensor is powered from a separate supply, its ground must be connected to the ESP32 ground for the digital data signal to work reliably.

### 3. Incorrect GPIO Assignment

Confirm that the sensor DATA pin is connected to the GPIO configured in the firmware. For the OLED, verify that the SDA and SCL connections match the configured I²C pins.

Some ESP32 pins have boot-strapping or other special functions. Review the specific board's pinout before using alternative GPIOs.

### 4. Missing DHT Pull-Up Resistor

A bare DHT sensor may require a pull-up resistor between VCC and DATA. Check whether the sensor is a standalone four-pin device or a breakout module with the required resistor already installed.

### 5. OLED Does Not Initialize

Check the OLED power connections, I²C wiring, display compatibility, and configured I²C address. The example uses `0x3C`, but some displays use a different address.

### 6. Invalid Sensor Readings

Verify the sensor type selected in the firmware, power supply, wiring, and data pin assignment. Check the Serial Monitor for sensor errors before investigating display-related issues.

### 7. Unstable or Intermittent Operation

Inspect loose jumper wires, incorrect power connections, and long or poorly organized wiring. Test each component separately before troubleshooting the complete system.

## Best Practices

- Keep jumper wires short and neatly routed.
- Label connections when working with multiple peripherals.
- Verify the sensor and display independently before integrating them.
- Check voltage and logic-level compatibility using the relevant module documentation.
- Use serial logging to identify initialization and sensor-reading failures.
- Avoid changing several hardware and firmware settings simultaneously during debugging.
- Add network and cloud functionality only after the basic measurement system is working reliably.

Organized wiring and incremental testing improve reliability and make problems easier to isolate.

## Future Improvements

The basic weather station can be extended with additional sensors and communication features.

### Wi-Fi-Based Remote Monitoring

Use the ESP32's built-in Wi-Fi to transmit environmental readings over HTTP or MQTT. A compatible server or IoT platform can collect the measurements for remote monitoring.

### Cloud Data Logging

Store sensor readings on a remote platform to observe environmental changes over time and analyze historical measurements.

### Additional Environmental Sensors

Possible additions include:

- Barometric pressure sensors.
- Rain sensors.
- Light-intensity sensors.
- Additional temperature and humidity sensors.
- Compatible air-quality sensors.

Each additional sensor requires appropriate interface, power, and firmware configuration.

### Battery or Solar Power

A standalone version could use a battery or solar-powered supply. This would require power-budget analysis and suitable sleep or duty-cycling strategies to reduce energy consumption.

### Improved Display Interface

The OLED interface could be expanded to show minimum and maximum values, measurement history, connection status, or other environmental parameters.

## Applications

This project can be adapted for:

- Indoor temperature and humidity monitoring.
- Home and office environmental monitoring.
- Classroom electronics demonstrations.
- Embedded systems and IoT learning.
- Environmental data collection prototypes.
- Development of larger wireless weather monitoring systems.

The basic implementation focuses on temperature and relative humidity. Additional measurements require compatible sensors and corresponding firmware changes.

## Reference

This project is based on the concepts discussed in the [Step-by-Step Guide to DIY ESP32 Weather Station](https://pcbcool.com/technical-guides/step-by-step-guide-to-diy-esp32-weather-station/), which covers the hardware components, sensor connections, and basic operation of an ESP32-based weather monitoring system.
