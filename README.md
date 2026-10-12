[![Language: Verilog](https://img.shields.io/badge/Language-Verilog%20%2F%20SystemVerilog-blue.svg)](https://en.wikipedia.org/wiki/Verilog)
[![Toolchain: EDA/FPGA](https://img.shields.io/badge/Tools-Questasim%20%7C%20Vivado-orange.svg)]()

# 🚗 Advanced Automotive ECU: Performance Tuning and Safety Systems

A collection of synthesizable, hardware-level automotive control modules designed in **Verilog/SystemVerilog** for FPGA deployment. This suite implements safety-critical vehicle dynamics, engine protection, dynamic aerodynamics, and driver assistance logic using a unified top-level ECU module that processes multiple sensory inputs concurrently without blocking or data hazards.

**Team Leader:** Amr Waheed Mohamed Abdallah

## 🌟 Motivation
We designed this Advanced Automotive ECU because it represents a cutting-edge, real-world application of digital logic in modern vehicle engineering. By implementing four distinct vehicle subsystems on a single FPGA—specifically optimizing control signals for a 200V DC motor system—we demonstrate how synchronous logic and rapid combinational circuits effectively manage critical mechanical functions.

---

## ⚙️ ECU Subsystems Overview & Architecture

### 1. 🅿️ Advanced Ultrasonic Parking Assist System
* **Proximity Calculation:** Uses a 16-bit sequential counter to measure the width of an incoming ultrasonic `echo_pulse`.
* **Dynamic Alerting:** Evaluates the pulse width; if it falls below the danger threshold of 5000 clock cycles, it instantly triggers an `alarm_led` warning of a dangerously close obstacle.

### 2. 🏎️ Motor Controller & Drive Modes (200V DC System)
* **Throttle Decoding:** A purely combinational decoder that maps a 2-bit throttle input to a duty cycle percentage (00: 25% Idle, 01: 50% Power, 10: 75% Power, 11: 100% Full Throttle).
* **PWM Generation:** Utilizes an 8-bit PWM generator counter to adjust the exact duty cycle for the 200V DC motor driver, outputting the `motor_pwm` signal.

### 3. 🛡️ RPM Limiter & Engine Protection (Launch Control)
* **Over-Rev Safeguard:** Features a hardware combinational comparator that continuously monitors an 8-bit `engine_rpm` input.
* **Safety Cut-off:** Instantly outputs an `ignition_cut` signal if the engine redline threshold (e.g., > 200 RPM) is exceeded, ensuring immediate engine protection.

### 4. 🌪️ Dynamic Active Aero System (Spoiler)
* **Speed-Dependent Deployment:** Employs an 8-bit vehicle speed monitoring comparator.
* **Automatic Trigger:** Generates a `spoiler_deploy` signal to automatically raise the rear aerodynamic wing when the vehicle's speed crosses a stability threshold (e.g., speed > 100).

---

## 🏗️ Top-Level Block Diagram

```text
+---------------------------------------------------------+
|               Advanced Automotive ECU (FPGA)            |
|                                                         |
| [clk] ---------------->|  +-------------------------+   |
| [rst] ---------------->|  | 1. Parking Assist       |   |
| [echo_pulse] --------->|  | - 16-bit Counter        |-------> [alarm_led]
|                        |  | - Comparator (< 5000)   |   |
|                        |  +-------------------------+   |
|                                                         |
| [throttle (2-bit)] --->|  +-------------------------+   |
|                        |  | 2. Motor Controller     |-------> [motor_pwm]
|                        |  | - 8-bit PWM Gen         |   | (To 200V DC Driver)
|                        |  | - Duty Cycle Decoder    |   |
|                        |  +-------------------------+   |
|                                                         |
| [engine_rpm (8-bit)] ->|  +-------------------------+   |
|                        |  | 3. RPM Limiter          |-------> [ignition_cut]
|                        |  | - Comparator (> 200)    |   |
|                        |  +-------------------------+   |
|                                                         |
| [vehicle_speed] ------>|  +-------------------------+   |
| (8-bit)                |  | 4. Active Aero System   |-------> [spoiler_deploy]
|                        |  | - Comparator (> 100)    |   |
