# ⚡ 3-Phase BLDC Inverter for Electric Vehicles

[![Hardware](https://img.shields.io/badge/Designed_in-KiCad_9.0-blue.svg)](https://www.kicad.org/)
[![Simulation](https://img.shields.io/badge/Simulated_in-LTspice-red.svg)]()
[![Firmware](https://img.shields.io/badge/Firmware-STM32CubeIDE-03234B.svg)]()

| 3D PCB Render | Manufactured PCB |
| :---: | :---: |
| ![3D Render](https://github.com/user-attachments/assets/56ca974c-490a-4645-a968-8297a346b8bb) | ![Hardware Validation](https://github.com/user-attachments/assets/f58c0a9f-1ea4-41f3-84ed-b0638082f68a) |

---

## 📌 Project Overview
This project details the end-to-end design, simulation, and hardware implementation of a complete 3-phase Brushless DC (BLDC) motor inverter. Tailored for low- to medium-voltage Electric Vehicle (EV) drivetrains (such as a 48V battery system), the system converts DC power into controlled 3-phase AC using a six-step trapezoidal commutation scheme.

*(Note: To protect proprietary engineering logic, the core circuit schematics are kept confidential and closed-source. Full PCB routing, 3D models, and physical hardware validation are detailed below.)*

### System Specifications
| Parameter | Component / Specification |
| :--- | :--- |
| **Microcontroller** | STM32F103C8T6 (Blue Pill) |
| **Gate Drivers** | IR2104 (High/Low-Side with bootstrap & dead-time) |
| **Power MOSFETs** | N-channel (Optimized for 48V systems) |
| **Current Sensing** | INA240A1 Precision Amplifier |
| **Control Logic** | 6-Step Trapezoidal Commutation (120° phase shift) |
| **PCB Stackup** | 4-Layer (Signal, Power Plane, Ground Plane, Signal) |

---

## ⚙️ Hardware Design & PCB Layout

The hardware architecture safely isolates the high-voltage power-switching devices from the sensitive low-voltage control circuits. 

### 1. Component Architecture
The logic operates via the STM32 generating PWM signals directed to three IR2104 gate drivers. An LM317 regulator provides stable +5V logic power, while the INA240A1 amplifier captures phase currents for monitoring and protection. 

### 2. 4-Layer PCB Routing & Hardware Implementation
To handle EV drivetrain currents and minimize Electromagnetic Interference (EMI), a 4-layer PCB was engineered:
* **Top Layer:** Control logic and low-voltage signal traces.
* **Inner Layer 1 (Power Plane):** Dedicated to distributing main battery voltage efficiently, reducing voltage drops.
* **Inner Layer 2 (Ground Plane):** Provides a common reference and suppresses electrical noise/crosstalk.
* **Bottom Layer:** Additional signal routing.

*Note: MOSFETs are aligned linearly along the board edge to allow for a single, unified heatsink block, coupled with an active cooling fan connector for high-load thermal management.*

| Complete 4-Layer Routing | Real-Life Hardware Operation |
| :---: | :---: |
| ![PCB Routing](https://github.com/user-attachments/assets/5829145a-b709-497c-b76d-8822ce7433ad) | ![Hardware Validation](https://github.com/user-attachments/assets/d2ae806b-6e60-452a-9f93-2b7279244b5f) |


---

## 💻 Firmware & Control Logic

The firmware was developed bare-metal using **STM32CubeIDE**, utilizing low-level hardware timers to achieve highly precise phase control without CPU bottlenecking.
* **PWM Generation:** Hardware timers generate synchronized PWM signals to drive the six MOSFETs.
* **Commutation:** Implements a strict 120° electrical phase shift logic to maintain a continuous, torque-producing rotating magnetic field.
* **Safety:** Manages dead-time insertion to prevent shoot-through shorts on the bridge arms.

---

## 📊 Simulation & Validation (LTspice)

Before committing to copper, the power stage was heavily simulated in LTspice to validate gate driving techniques, bootstrap capacitor behavior, and switching losses. 

**Simulation Results:**
* Successfully verified the exact 120° spacing between phase voltages.
* Validated trapezoidal phase current waveforms consistent with BLDC operation.
* Calculated total power losses across the bridge at ~15.5 W, informing heatsink requirements.

| Schematic Simulation | $120^{\circ}$ Phase Shift Waveforms |
| :---: | :---: |
| ![LTspice Circuit](https://github.com/user-attachments/assets/c9be645e-5318-4802-b05c-ef15e1e0e8f0) | ![Waveforms](https://github.com/user-attachments/assets/1244a294-cbb2-4e0e-b22f-b91cf3b405e8) |

---

## 🚀 Conclusion
This system successfully demonstrates a robust, scalable architecture for electric vehicle motor control. By integrating hardware-level timer synchronization with a heavily optimized 4-layer power routing design, the inverter achieves high efficiency and thermal stability under load.
