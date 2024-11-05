![image](https://github.com/user-attachments/assets/e8978d20-c5d4-4e09-a5d5-d8dd63e53b35)
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

Here’s a quick look at how we’re mapping the stock Honda ECU connector (179686-6) to the Teensy 4.1:

| Honda ECU Pin | Function                             | Teensy Pin | Function                       | Signal Type                 |
|---------------|--------------------------------------|------------|--------------------------------|-----------------------------|
| A1            | Injector #4                          | D6         | Digital output (Inj 4)         | Low side, 1.5A (Active Low) |
| A2            | Injector #3                          | D7         | Digital output (Inj 3)         | Low side, 1.5A (Active Low) |
| A3            | Injector #2                          | D8         | Digital output (Inj 2)         | Low side, 1.5A (Active Low) |
| A4            | Injector #1                          | D9         | Digital output (Inj 1)         | Low side, 1.5A (Active Low) |
| A11           | Ignition Power                       | VIN        | Power Input                    | Key-On Power                 |
| A16           | Fuel Pump Relay Control              | D10        | Digital output (Fuel Pump)     | Low side, Relay Control      |
| A18           | MIL (Malfunction Indicator Light)    | D11        | Digital output (MIL)           | Low side, Lamp Control       |
| A20           | Ignition Control Module              | D12        | Digital output (Ignition Ctrl) | Inverted dwell control       |
| A24           | Ignition Power #2                    | VIN        | Power Input                    | Key-On Power                 |
| A27           | Radiator Fan Control                 | D13        | Digital output (Fan Control)   | Low side, Relay Control      |
| C1            | Crank Fluctuation Sensor Positive    | A0         | Analog input (Crank Signal)    | VR+                          |
| C2            | Crank Position Sensor Positive       | A1         | Analog input (Crank Pos)       | VR+                          |
| C3            | Top Dead Center Sensor Positive      | A2         | Analog input (TDC Signal)      | VR+                          |
| D1            | TPS (Throttle Position Sensor)       | A3         | Analog input (TPS)             | 0-5V Analog                  |
| D2            | ECT (Engine Coolant Temp Sensor)     | A4         | Analog input (ECT)             | 0-5V Analog                  |
| D3            | MAP (Manifold Absolute Pressure)     | A5         | Analog input (MAP)             | 0-5V Analog                  |
| D7            | Primary Oxygen Sensor Signal         | A6         | Analog input (O2)              | 0-5V Analog                  |
| D8            | IAT (Intake Air Temperature)         | A7         | Analog input (IAT)             | 0-5V Analog                  |


This mapping is the backbone of our custom ECU, connecting the Teensy 4.1 to every critical sensor, injector, and ignition signal. With these mappings, our ECU has full control over fuel, spark, and monitoring.

---

Let’s bring new life to the Honda D-series engine! Feel free to contribute or give feedback – together, we can create a custom ECU that maximizes the potential of these engines.
