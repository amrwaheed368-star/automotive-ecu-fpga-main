[![Language: Verilog](https://img.shields.io/badge/Language-Verilog%20%2F%20SystemVerilog-blue.svg)](https://en.wikipedia.org/wiki/Verilog)
[![Toolchain: EDA/FPGA](https://img.shields.io/badge/Tools-Questasim%20%7C%20Vivado-orange.svg)]()

# 🚗 FPGA Automotive Subsystems Control Suite

A collection of synthesizable, hardware-level automotive control modules designed in **Verilog/SystemVerilog** for FPGA deployment. This suite implements safety-critical vehicle dynamics, engine protection, dynamic aerodynamics, and driver assistance logic alongside modular self-checking testbenches.

Detailed design calculations, block diagrams, and state machine specifications are documented in [`docs/project_report.pdf`](docs/project_report.pdf)

---

## 🌟 ECU Subsystems Overview

### 1. 🛑 Anti-lock Braking System (ABS) & Traction Control
* **Wheel Slip Calculation:** Computes real-time slip ratios by comparing wheel speed metrics against vehicle velocity.
* **PWM Torque Management:** Generates dynamic Pulse-Width Modulation (PWM) signals to reduce engine torque or regulate braking during wheel slip or lockup.

### 2. 🛡️ Smart RPM Limiter & Engine Protection
* **Over-Rev Safeguard:** Tracks real-time engine RPM metrics and triggers automatic safety cut-offs when reaching critical thresholds.
* **Safety State Machine:** Enforces soft-limits during abnormal operational or thermal states.

### 3. 🏎️ Dynamic Active Spoiler System
* **Speed-Dependent Deployment:** Finite State Machine (FSM) that adjusts rear aerodynamic spoiler angle based on vehicle speed stages.
* **Emergency Air-Brake:** Maximizes downforce dynamically under hard deceleration events.

### 4. 🅿️ Advanced Ultrasonic Parking Assist
* **Proximity Calculation:** Reads echo timing signals from ultrasonic sensors to determine obstacle distance.
* **Dynamic Alerting:** Triggers progressive visual and audible alert patterns based on distance zone boundaries.
### 5. ⚙️ Multi-Mode Drive Controller
* **Selectable Drive Maps:** Maps pedal/throttle input to target duty cycles across Eco, Normal, Sport, and Track modes via custom lookup tables (LUTs).
* **Rate-Limited Throttle Transitions:** Implements synchronous slewing rate-limiters to prevent abrupt power spikes, ensuring smooth transitions and drivetrain protection.
* **Traction Control Interfacing:** Exposes real-time duty cycle metrics directly for dynamic torque management integration.
---

## 📂 Folder Layout

* **[`rtl/`](rtl/)**: Synthesizable Verilog/SystemVerilog modules for individual automotive control units.
* **[`tb/`](tb/)**: Independent, self-checking testbenches with targeted stimulus injection for each subsystem.
* **[`docs/`](docs/)**: Comprehensive technical report ([`project_report.pdf`](docs/project_report.pdf)) detailing hardware specs and simulation analysis.

---

## 🛠️ Tools & Simulation

* **Language:** Verilog / SystemVerilog
* **Design Methodology:** Synthesizable RTL, Synchronous FSM Design
* **Simulation & EDA Tools:** ModelSim / Questasim / Vivado / Icarus Verilog & GTKWave

### Running Individual Subsystem Simulations (Icarus Verilog Example)

You can simulate any specific module alongside its matching testbench:

```bash
# Example: Simulating the ABS & Traction Control Module
iverilog -o abs_sim tb/tb_abs_traction_control.v rtl/abs_traction_control.v
vvp abs_sim

# View waveform
gtkwave abs_sim.vcd
