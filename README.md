
# Honda D-Series Custom ECU

## Project Overview
The goal of this project is to design a custom ECU for a Honda D-series engine, initially supporting a basic naturally aspirated (NA) setup. The ECU will control the engine's fuel injection, ignition timing, and key sensors, replacing the stock OBD1 ECU. Future upgrades are planned for a turbocharged build, ethanol (E85) fuel support, and additional features such as boost control and coil-on-plug ignition.

This custom ECU board have the goal of providing the necessary processing power, flexibility, and I/O for engine management. The design will follow a modular approach, allowing future expansion to support more advanced features.

## Phase 1: Basic ECU Design (NA Setup)

### Key Features
The initial design will focus on the core functionality to control a naturally aspirated engine setup:
- **Fuel Injection Control** (Batch or semi-sequential injection for a 4-cylinder)
- **Ignition Timing Control** (Using stock Honda distributor with igniter)
- **Sensor Inputs** (MAP, TPS, IAT, ECT, O2 sensors)
- **Idle Air Control** (2-wire or 3-wire IACV)
- **Power Supply and Signal Conditioning** (for stable sensor inputs and outputs)

### Teensy 3.5/3.6 Selection
- **Microcontroller**: Teensy 3.5/3.6
  - Clock Speed: 120 MHz (Teensy 3.5), 180 MHz (Teensy 3.6)
  - 3.3V logic with support for 5V-tolerant inputs.
  - Ample I/O pins for connecting sensors, injectors, ignition, and other components.

### Why Teensy 3.5/3.6?
- More powerful than the Arduino Mega, which is commonly used in Speeduino projects.
- Provides faster processing, allowing future upgrades like sequential fuel injection, turbo boost control, and ethanol compensation.
- Offers enough I/O to handle the 4-cylinder engine and future features.

---

## Selected Components for Basic Setup

### 1. Fuel Injection Control
- **Injection Mode**: Batch injection (two injectors fire at once, simplifying wiring and control).
- **Injector Drivers**: MOSFETs will handle the switching of fuel injectors, driven by GPIO pins on the Teensy.
  - **MOSFET Type**: Logic-level MOSFETs (e.g., IRLZ44N), capable of driving stock high-impedance injectors.
  
### 2. Ignition Control
- **Distributor Setup**: The stock Honda OBD1 distributor with an internal igniter is used for ignition timing control.
  - The Teensy will trigger the igniter module with a logic-level pulse, which in turn drives the ignition coil.
  - **VR Sensor Signal Conditioning**: A VR conditioner circuit (e.g., MAX9926) will be used to process crank and cam signals from the distributor.https://mcs.woodward.com/content/motohawk/Documentation/MotoHawk2015bSP0/HTML/MotoHawk_topics/VRInterfacing.html
  
- No MOSFETs are required for this setup, as the igniter handles high-current switching for the ignition coil.

### 3. Sensors
- **MAP Sensor**: Used to monitor manifold pressure for fuel calculations. The stock 1-bar MAP sensor will be used initially.
- **Throttle Position Sensor (TPS)**: A potentiometer-type sensor to measure throttle position.
- **Intake Air Temperature Sensor (IAT)**: Measures intake air temperature for accurate fuel adjustments.
- **Engine Coolant Temperature Sensor (ECT)**: Monitors engine temperature to manage cold-start enrichment and fan control.
- **Oxygen Sensor (O2)**: A narrowband sensor is used for monitoring air-fuel ratio (AFR). A wideband sensor will be added in future turbo/E85 setups.

### 4. Idle Air Control Valve (IACV)
- **IACV Control**: The ECU will control the IACV using PWM (for 2-wire valves) or stepper control (for 3-wire valves) based on the specific hardware in the vehicle.

### 5. Power Supply and Signal Conditioning
- **Power Regulation**: A stable 5V power supply will be designed to power the Teensy, sensors, and other components.
- **Flyback Diodes**: Used across injector driver circuits to protect against voltage spikes.
- **Signal Conditioning**: Appropriate resistors and capacitors will be used to clean sensor signals, ensuring reliable readings.

---

## Interface: Honda ECU Plug to Teensy 3.5 Pinout Mapping

This section shows the pin mapping between the stock Honda ECU connector (TE-3-178780-76P) and the Teensy 3.5, along with the signal type for each pin.

|   Honda ECU Pin |  Function                  |    Teensy Pin |  Function               | Signal Type                   |
|-----------------|----------------------------|---------------|-------------------------|--------------------------------|
|   A7            |  TPS (Throttle Position)    |   A0          |  Analog input (TPS)      | Positive (0-5V)               |
|   D17           |  MAP Sensor                 |   A1          |  Analog input (MAP)      | Positive (0-5V)               |
|   A8            |  ECT (Coolant Temp)         |   A2          |  Analog input (ECT)      | Positive (0-5V)               |
|   D14           |  IAT (Intake Air Temp)      |   A3          |  Analog input (IAT)      | Positive (0-5V)               |
|   C2            |  CKP (Crankshaft Position)  |   D2          |  Digital input (CKP)     | Positive Pulse                |
|   C1            |  TDC (Top Dead Center)      |   D3          |  Digital input (TDC)     | Positive Pulse                |
|   D10           |  O2 Sensor (Narrowband)     |   A9          |  Analog input (O2)       | Positive (0-5V)               |
|   C11           |  VSS (Vehicle Speed Sensor) |   D4          |  Digital input (VSS)     | Positive Pulse                |
|   B16           |  Ignition Signal            |   D5          |  Digital output (Ignition)| Negative (Active Low)          |
|   A11           |  Injector 1 (Cyl 1)         |   D6          |  Digital output (Inj 1)  | Negative (Active Low)          |
|   A12           |  Injector 2 (Cyl 2)         |   D7          |  Digital output (Inj 2)  | Negative (Active Low)          |
|   A14           |  Injector 3 (Cyl 3)         |   D8          |  Digital output (Inj 3)  | Negative (Active Low)          |
|   A15           |  Injector 4 (Cyl 4)         |   D9          |  Digital output (Inj 4)  | Negative (Active Low)          |
|   A9            |  IACV                       |   D10         |  Digital output (IACV)   | Positive (Active High)         |
|   P20           |  Fuel Pump Control          |   D11         |  Digital output (Fuel Pump Relay) | Negative (Active Low) |
|   A4            |  VTEC Solenoid              |   D12         |  Digital output (VTEC)   | Positive (Active High)         |

---

This layout covers the critical pin mappings for the custom ECU using the Teensy 3.5/3.6, establishing a direct interface with the stock Honda ECU plug. These mappings will be the foundation for the ECU's control over engine sensors, fuel injectors, and ignition.
