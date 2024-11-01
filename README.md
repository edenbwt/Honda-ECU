## Project Overview 🌟

Welcome to the **Honda D-Series Custom ECU** project! This project is dedicated to designing a feature-rich, custom ECU for Honda's beloved D-series engines. We’re starting with a basic **naturally aspirated (NA) setup** and planning to expand for **turbocharging, ethanol (E85) fuel**, and advanced features like **boost control and coil-on-plug ignition**.

Our goal: to create a powerful, flexible, and modular ECU that can evolve alongside your Honda build.

---

## 🎯 Phase 1: Basic ECU Design (NA Setup)

To start, we’re focusing on the essential components needed to run a naturally aspirated D-series engine:

### Key Features
- **Fuel Injection Control** – Supports batch or semi-sequential injection for smooth performance.
- **Ignition Timing Control** – Utilizing the stock Honda distributor and internal igniter for precise timing.
- **Sensor Inputs** – Connects to MAP, TPS, IAT, ECT, and O2 sensors for engine monitoring and control.
- **Idle Air Control (IAC)** – Control for either 2-wire or 3-wire IACVs.
- **Power Supply & Signal Conditioning** – Ensures reliable and stable signal processing.

---

## 🧠 Teensy 4.1 MCU Selection

For this ECU project, we've chosen the **Teensy 4.1** as our microcontroller. The Teensy 4.1 offers significant improvements over previous versions, making it ideal for both current and future expansion:

- **Increased Power & Speed** – With a 600 MHz processing speed, the Teensy 4.1 provides faster, smoother performance than the Teensy 3.5/3.6.
- **Ample I/O and Expandability** – It has sufficient I/O to handle a 4-cylinder engine now and more advanced features as we expand.
- **3.3V Logic Compatibility** – Compatible with both 3.3V and 5V inputs, ensuring flexibility across various sensor setups.

This board sets us up to handle sequential injection, turbo control, ethanol adjustments, and more advanced functionality in the future. ⚡

---

## Components for the Basic NA Setup

### 1. Fuel Injection Control 💧
- **Injection Mode**: We'll start with **batch injection**, which simplifies wiring by firing two injectors at once.
- **Injector Drivers**: **Logic-level MOSFETs** (like the IRLZ44N) will control the injectors. These MOSFETs are perfect for handling high-impedance injectors while remaining easy to interface with the Teensy.

### 2. Ignition Control 🔥
- **Honda Distributor**: Our setup uses the stock Honda OBD1 distributor with its built-in igniter for ignition timing.
- **Custom VR Conditioner**: The **basic VR conditioner from Speeduino doesn’t work well with Honda distributors**, so we’re developing our own **Honda-specific VR conditioner** for reliable crank and cam signal processing.
  - This custom solution will ensure clean and consistent signals, allowing the Teensy to control ignition timing accurately.

### 3. Sensors 📡
We’ll connect to the following sensors:
- **MAP Sensor** – Measures manifold pressure for fuel calculations.
- **Throttle Position Sensor (TPS)** – Monitors throttle position to determine engine load.
- **Intake Air Temperature Sensor (IAT)** – Tracks intake air temp to adjust fuel mix.
- **Engine Coolant Temperature Sensor (ECT)** – Used for cold-start enrichment and fan control.
- **Oxygen Sensor (O2)** – We’re starting with a narrowband sensor for basic AFR monitoring; a wideband sensor will come with the turbo/E85 setup.

### 4. Idle Air Control Valve (IACV) ⚙️
- The ECU will control the IACV using **PWM for 2-wire valves** or **stepper control for 3-wire valves**, allowing precise idle control regardless of valve type.

### 5. Power Supply & Signal Conditioning 🔋
- **Power Regulation**: We’ll create a stable 5V power supply to run the Teensy and sensors without any voltage fluctuations.
- **Flyback Diodes**: Protect injector circuits against voltage spikes for durability.
- **Signal Conditioning**: Adding resistors and capacitors will help ensure clean sensor signals for smooth engine control.

---

## 🔌 Honda ECU Plug to Teensy 4.1 Pinout Mapping

Here’s a quick look at how we’re mapping the stock Honda ECU connector (TE-3-178780-76P) to the Teensy 4.1:

| Honda ECU Pin | Function                  | Teensy Pin | Function                 | Signal Type               |
|---------------|---------------------------|------------|--------------------------|---------------------------|
| A7            | TPS (Throttle Position)   | A0         | Analog input (TPS)       | Positive (0-5V)           |
| D17           | MAP Sensor                | A1         | Analog input (MAP)       | Positive (0-5V)           |
| A8            | ECT (Coolant Temp)        | A2         | Analog input (ECT)       | Positive (0-5V)           |
| D14           | IAT (Intake Air Temp)     | A3         | Analog input (IAT)       | Positive (0-5V)           |
| C2            | CKP (Crankshaft Position) | D2         | Digital input (CKP)      | Positive Pulse            |
| C1            | TDC (Top Dead Center)     | D3         | Digital input (TDC)      | Positive Pulse            |
| D10           | O2 Sensor (Narrowband)    | A9         | Analog input (O2)        | Positive (0-5V)           |
| C11           | VSS (Vehicle Speed Sensor)| D4         | Digital input (VSS)      | Positive Pulse            |
| B16           | Ignition Signal           | D5         | Digital output (Ignition)| Negative (Active Low)     |
| A11           | Injector 1 (Cyl 1)        | D6         | Digital output (Inj 1)   | Negative (Active Low)     |
| A12           | Injector 2 (Cyl 2)        | D7         | Digital output (Inj 2)   | Negative (Active Low)     |
| A14           | Injector 3 (Cyl 3)        | D8         | Digital output (Inj 3)   | Negative (Active Low)     |
| A15           | Injector 4 (Cyl 4)        | D9         | Digital output (Inj 4)   | Negative (Active Low)     |
| A9            | IACV                      | D10        | Digital output (IACV)    | Positive (Active High)    |
| P20           | Fuel Pump Control         | D11        | Digital output (Fuel Pump Relay) | Negative (Active Low) |
| A4            | VTEC Solenoid             | D12        | Digital output (VTEC)    | Positive (Active High)    |

This mapping is the backbone of our custom ECU, connecting the Teensy 4.1 to every critical sensor, injector, and ignition signal. With these mappings, our ECU has full control over fuel, spark, and monitoring.

---

Let’s bring new life to the Honda D-series engine! Feel free to contribute or give feedback – together, we can create a custom ECU that maximizes the potential of these engines.
