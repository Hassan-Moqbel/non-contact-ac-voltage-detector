# Non-Contact AC Line Checker

![Instrumentation & Sensors](https://img.shields.io/badge/Domain-Instrumentation_%26_Sensors-FF6F00?style=for-the-badge)
![Capacitive Field Sensing](https://img.shields.io/badge/Topology-Capacitive_Sensing-009999?style=for-the-badge)
![Darlington Amplification](https://img.shields.io/badge/Circuit-Darlington_Amplification-4B0082?style=for-the-badge)
![Discrete Electronics](https://img.shields.io/badge/Domain-Discrete_Electronics-28A745?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

## Executive Overview
Identifying live 220V/110V AC conductors safely is a critical requirement in electrical maintenance. This project details the design and hardware verification of a **Non-Contact AC Line Checker**. Operating entirely on discrete components, the instrument exploits the principle of capacitive coupling. By utilizing a high-gain cascaded Darlington transistor array, the device successfully detects the rapidly alternating electromagnetic field of a live AC wire through its insulation, triggering audio-visual feedback without requiring physical ohmic contact with bare copper.

> [!CAUTION]
> **High Voltage Mains Safety & Insulation Callout**
> While this device is designed to be "non-contact," testing it requires bringing the antenna in close proximity to live 220V/110V AC mains conductors. Ensure the physical insulation of the wire under test is completely intact. **Do not use this circuit as a definitive life-safety tool.** A non-contact voltage tester (NCVT) cannot verify a circuit is dead; it can only indicate the presence of an AC field. Always confirm zero-energy states with a certified contact multi-meter before touching electrical terminals.

## System Highlights
- **Zero-Contact Electrical Sensing**: Detects live alternating current (AC) fields through plastic conduit or wire insulation via an open-ended antenna.
- **Multi-Stage Darlington High-Gain Amplification**: Cascades three NPN bipolar junction transistors to multiply a microscopic base current into a sufficient collector drive current.
- **Audio-Visual Feedback Indicators**: Instantly alerts the user to the presence of a live field utilizing a bright LED and an active buzzer.
- **Compact Portable Form Factor**: Entirely battery-operated (9V DC) allowing for safe, isolated, and portable use.

## System Architecture Diagram

```mermaid
flowchart LR
    AC["50/60Hz Live AC Conductor 220V"] -->|Capacitive Coupling \nE-field / C_air| ANT["Probe Antenna / Wire Loop"]
    ANT -->|Pico-amp I_disp| Q1["Transistor Q1 Base"]
    Q1 -->|Current Gain β1| Q2["Transistor Q2 Base"]
    Q2 -->|Current Gain β2| Q3["Transistor Q3 Base"]
    Q3 -->|High Drive Current| LOAD["Current-Limiting Network"]
    LOAD --> OUT["Indicator Node \nLED & Buzzer Output"]
```

## Theoretical & Mathematical Models

### 1. Capacitive Coupling & Displacement Current Formulation
The antenna acts as one plate of a capacitor, with the live AC wire acting as the other, separated by an air/insulation dielectric ($C_{air}$). The alternating voltage ($V_{ac}$) induces a displacement current ($I_{disp}$) into the antenna:
$$I_{disp} = C_{air} \frac{dV_{ac}}{dt} = 2\pi f C_{air} V_{peak} \cos(2\pi f t)$$
*(This equation demonstrates why higher grid frequencies or sharper transient spikes induce higher displacement currents, increasing sensitivity).*

### 2. Cascaded Multi-stage Darlington Current Gain
Because $I_{disp}$ is in the pico-ampere range, a single transistor cannot drive an LED. By cascading three transistors, the total current gain ($\beta_{total}$) becomes the product of individual gains:
$$\beta_{total} \approx \beta_1 \cdot \beta_2 \cdot \beta_3$$
Assuming $\beta \approx 100$ for a standard BC547, the total gain approaches $1,000,000$. The final collector current is:
$$I_C = \beta_{total} \cdot I_{disp}$$
*(This amplifies the pico-ampere capacitive currents into milli-amperes to drive the indicators).*

### 3. Base-Emitter Threshold Turn-on Constraint
For the Darlington array to conduct, the induced voltage must overcome the sum of all three base-emitter junction drops:
$$V_{trigger} \ge V_{BE1} + V_{BE2} + V_{BE3} \approx 3 \times 0.65\text{V} \approx 1.95\text{V}$$

## Hardware Bill of Materials (BOM)
| Component | Specification / Function |
| :--- | :--- |
| **NPN Transistors (x3)** | BC547 or 2N3904 (Cascaded for high $\beta_{total}$) |
| **Probe Antenna** | Coiled solid-core copper wire (captures E-field) |
| **Visual Indicator** | $5\text{mm}$ Red LED |
| **Acoustic Indicator** | 5V - 9V Active Piezo Buzzer |
| **Current Limiter** | $220\Omega - 1\text{k}\Omega$ Resistor |
| **Power Source** | 9V DC Battery |

## Sensitivity Calibration & Field Tuning Guide
The sensitivity of the checker is directly proportional to the surface area of the antenna and the combined $\beta$ of the transistors. 
1. **To increase sensitivity** (e.g., detecting wires buried in a wall), increase the number of coils on the copper antenna to increase $C_{air}$.
2. **To decrease sensitivity** (e.g., isolating a single wire in a dense bundle without false triggering), shorten the antenna probe to a tiny straight tip.

## Authentic Media Catalog
- **Engineering Report**: [`docs/A non-contact_AC_line_checkerحسن+موسى.pdf`](docs/)
- **Original Schematics & Prototype Evidence**: Located in [`media/photos/`](media/photos/) as **[ORIGINAL HARDWARE & SCHEMATIC ARTIFACTS]**.
- **Demonstration Video**: Available in [`media/videos/`](media/videos/) as **[ORIGINAL SENSING TEST VIDEO]**.

## Engineering Audit & Limitations
- **Triboelectric False Triggers**: Because the input impedance of a 3-stage Darlington pair is exceptionally high, simple static electricity (rubbing against a sleeve or plastic tube) will trigger the circuit. Adding a high-value pull-down resistor (e.g., $10\text{M}\Omega$) from the first base to ground can bleed off static charge but severely limits AC sensitivity.
- **Shielded Conduit Limitations**: This device detects purely electrostatic fields. If the AC wire is run inside a grounded metallic conduit (e.g., EMT or BX cable), the E-field is blocked by the Faraday cage effect, and the checker will fail to detect the live voltage.
- **Lack of RF Shielding**: The extreme gain makes the circuit susceptible to nearby high-power RF sources (like transmitting cell phones).

---

**Hassan Moqbel Morshed Ghaleb**
Mechatronics Engineer | Mechanical Design & CAD (SolidWorks & AutoCAD) | Preventive Maintenance & Electromechanical Systems | Industrial Automation, Control Systems, Robotics & Intelligent Machines | CAD/FEA, Embedded Systems, Python & C++
[GitHub](https://github.com/Hassan-Moqbel) · [Facebook](https://www.facebook.com/share/1BqxAgVjHi/) · [LinkedIn](https://www.linkedin.com/in/hassan-moqbel)

## License
This project is licensed under the [MIT License](LICENSE).
