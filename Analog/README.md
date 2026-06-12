<div align="center">

# Analog & VLSI Circuit Design Projects

### Transistor Characterization • Analog Circuit Design • Reference Circuit Design • CMOS Layout Verification

![Cadence Virtuoso](https://img.shields.io/badge/Cadence-Virtuoso-blue?style=for-the-badge)
![LTspice](https://img.shields.io/badge/LTspice-Simulation-red?style=for-the-badge)
![gpdk090](https://img.shields.io/badge/PDK-gpdk090-green?style=for-the-badge)
![TSMC 180nm](https://img.shields.io/badge/Process-TSMC%20180nm-orange?style=for-the-badge)
![DRC LVS](https://img.shields.io/badge/Verification-DRC%20%7C%20LVS-purple?style=for-the-badge)

</div>

---

## Project Overview

This repository contains a collection of four Analog IC and VLSI design projects. The projects cover the complete learning path from basic MOSFET model characterization to analog circuit simulation, reference voltage generation, and custom CMOS layout verification.

The main objective of this repository is to document practical circuit-design work using industry-style EDA tools such as Cadence Virtuoso, LTspice, gpdk090 PDK, TSMC 180nm models, and Assura DRC/LVS verification.

---

## Project Dashboard

| No. | Project | Design Area | Tools / Technology | Main Outcome |
| --- | --- | --- | --- | --- |
| 01 | MOSFET Characterization Suite | Device characterization | LTspice 26, TSMC 180nm BSIM3 | NMOS/PMOS Id, gm, gm/Id and small-signal behavior analyzed |
| 02 | Voltage-Controlled Phase Shifter | Analog signal processing | Cadence Virtuoso, gpdk090 nmos1v | Positive phase shift from about 1.18° to 90.66° at 10 kHz |
| 03 | Bandgap Reference Circuit | Analog reference design | Cadence Virtuoso, gpdk090 | 1.45 V reference with about 14.2 ppm/°C temperature coefficient |
| 04 | CMOS Inverter Layout | Physical layout verification | Cadence Virtuoso Layout Suite, gpdk090, Assura | Custom layout passed DRC and LVS verification |

---

# 01. MOSFET Characterization Suite

## Summary

The MOSFET Characterization Suite analyzes NMOS and PMOS transistor behavior using the TSMC 180nm BSIM3 model in LTspice. The purpose of this project is to build a practical design reference for analog circuit sizing, biasing, gain estimation, and gm/Id-based design.

Instead of depending only on textbook MOSFET equations, this project extracts useful transistor behavior directly from simulation.

## What Was Done

- Swept **Id vs Vgs** to observe threshold behavior.
- Swept **Id vs Vds** to identify triode and saturation regions.
- Plotted **gm vs Vgs** to study transconductance behavior.
- Plotted **gm/Id vs Vgs** for low-power and high-speed bias selection.
- Used operating-point extraction to observe small-signal parameters.

## Key Results

| Parameter | NMOS | PMOS |
| --- | --- | --- |
| Approx. threshold voltage | 0.35 V | 0.42 V |
| Peak gm | 4.0 mA/V | 2.2 mA/V |
| gm/Id range | ~27 V⁻¹ to ~4 V⁻¹ | Similar gm/Id trend |
| Technology | TSMC 180nm BSIM3 | TSMC 180nm BSIM3 |

## Design Insight

This project shows why transistor characterization is important before designing amplifiers or analog blocks. The gm/Id plot helps select whether the circuit should operate in a low-power region, moderate-inversion region, or high-speed strong-inversion region.

---

# 02. Voltage-Controlled Phase Shifter

## Summary

This project implements a voltage-controlled positive phase shifter using a first-order active all-pass filter structure. The circuit uses a VCVS-based active amplifier and a gpdk090 NMOS transistor as a voltage-controlled resistor.

The target was to control the phase shift from approximately 0° to 90° using a control voltage from 0 V to 1 V.

## Circuit Concept

An all-pass filter was selected because it changes the phase of a signal while keeping the gain almost constant. The final design was modified from an initial negative-phase topology to a positive-phase topology so that the output leads the input.

## Final Design Values

| Component / Parameter | Final Value |
| --- | --- |
| Circuit type | Positive first-order active all-pass phase shifter |
| Active element | VCVS, gain = 100k |
| NMOS model | gpdk90 nmos1v |
| NMOS W/L | 1.2 µm / 1 µm |
| R1, R2 | 100 kΩ, 100 kΩ |
| C0 | 100 pF |
| Rseries | 153 kΩ |
| Rbias | 100 MΩ |
| Control voltage | 0 V to 1 V |
| Design frequency | 10 kHz |

## Main Result

| Vctrl | Phase Shift at 10 kHz |
| --- | --- |
| 0 V | ~1.18° |
| 1 V | ~90.66° |

The gain remained close to 0 dB, confirming the all-pass behavior of the circuit.

## Important Note

A supply-powered op-amp implementation was attempted, but the Cadence/Spectre environment produced an AHDL compile error for the `ahdlLib` op-amp model. Therefore, the final verified simulation used the VCVS model.

---

# 03. Bandgap Reference Circuit

## Summary

This project designs and simulates a 1.45 V bandgap reference circuit in Cadence Virtuoso using the gpdk090 PDK. The goal was to generate a stable reference voltage over temperature by combining CTAT and PTAT voltage behavior.

The design uses a PMOS current mirror, PNP bipolar devices, resistor tuning, and an ideal VCVS-based op-amp.

## Design Concept

A bandgap reference works by adding two temperature-dependent terms:

- **CTAT voltage**: BJT VBE decreases as temperature increases.
- **PTAT voltage**: ΔVBE increases as temperature increases.

By balancing these two slopes, the output voltage becomes nearly constant over temperature.

## Final Design Summary

| Block / Parameter | Final Value |
| --- | --- |
| Supply voltage | 3.3 V |
| Core resistors | R1 = 1 kΩ, R2 = 6.85 kΩ |
| BJT multiplier ratio | 1 : 10 : 1 |
| PMOS size | W = 2.4 µm, L = 500 nm |
| Ideal op-amp gain | 100000 |
| Output scaling stage | Non-inverting gain stage |
| Gain-stage resistors | Rg = 100 kΩ, Rf = 24 kΩ |

## Main Result

| Parameter | Result |
| --- | --- |
| Final VREF range | ~1.4486 V to ~1.4520 V |
| Nominal reference voltage | ~1.45 V |
| Temperature range | -40°C to 125°C |
| Estimated temperature coefficient | ~14.2 ppm/°C |

The design satisfies the required 1.4 V to 1.5 V output range and the 10 ppm/°C to 17 ppm/°C temperature coefficient target.

---

# 04. CMOS Inverter Layout

## Summary

This project completes the custom physical layout of a CMOS inverter in Cadence Virtuoso using gpdk090 devices. The project includes schematic design, manual layout design, pin creation, body connection, DRC verification, and LVS verification.

The layout was verified using Assura DRC and Assura LVS.

## Design Details

| Item | Details |
| --- | --- |
| Circuit | CMOS inverter |
| PMOS device | pmos1v |
| NMOS device | nmos1v |
| PMOS width | 480 nm |
| NMOS width | 240 nm |
| Channel length | 100 nm |
| Layout pins | A, Y, VDD, VSS |
| Verification tool | Assura DRC and Assura LVS |

## Layout Flow

- Created the CMOS inverter schematic.
- Placed PMOS at the top and NMOS at the bottom.
- Connected PMOS source/body to VDD.
- Connected NMOS source/body to VSS.
- Connected PMOS and NMOS drains together as output Y.
- Connected both gates together as input A.
- Added layout pins matching the schematic pin names.
- Ran DRC and LVS verification.

## Final Verification Result

| Verification | Result |
| --- | --- |
| DRC | Passed, no DRC errors found |
| LVS | Passed, schematic and layout matched |
| Net mismatches | 0 |
| Device mismatches | 0 |
| Pin mismatches | 0 |
| Parameter mismatches | 0 |

This confirms that the final layout is both physically valid and electrically equivalent to the schematic.

---

## Skills Demonstrated

| Skill Area | Demonstrated Through |
| --- | --- |
| MOSFET characterization | Id-Vgs, Id-Vds, gm, gm/Id and operating-point analysis |
| Analog circuit design | Phase shifter and bandgap reference design |
| Biasing and sizing | NMOS resistor tuning, PMOS mirror sizing, resistor-ratio tuning |
| Temperature analysis | Bandgap reference simulation from -40°C to 125°C |
| Physical layout | CMOS inverter custom layout in Cadence Virtuoso |
| Verification | Assura DRC and LVS checks |
| EDA tools | LTspice, Cadence Virtuoso, ADE, ViVA, Assura |

---

## Repository Structure

```text
.
├── MOSFET_Characterization_Suite/
│   ├── NMOS/
│   ├── PMOS/
│   ├── images/
│   └── report/
│
├── Voltage_Controlled_Phase_Shifter/
│   ├── cadence_files/
│   ├── images/
│   └── report/
│
├── Bandgap_Reference_Circuit/
│   ├── cadence_files/
│   ├── images/
│   └── report/
│
├── CMOS_Inverter_Layout/
│   ├── cadence_files/
│   ├── images/
│   └── report/
│
└── README.md
```

---

## How to Use This Repository

1. Open the individual project folder.
2. Read the project report from the `report/` folder for theory, design steps, simulation setup, and results.
3. Open the simulation or Cadence project files from the corresponding project folder.
4. Make sure the required technology files or PDKs are available before running the simulations.

### Tool Requirements

| Project | Required Tool / Setup |
| --- | --- |
| MOSFET Characterization Suite | LTspice 26 with TSMC 180nm BSIM3 model files |
| Voltage-Controlled Phase Shifter | Cadence Virtuoso with gpdk090 |
| Bandgap Reference Circuit | Cadence Virtuoso with gpdk090 |
| CMOS Inverter Layout | Cadence Virtuoso Layout Suite with gpdk090 and Assura |

---

## Future Improvements

- Replace ideal VCVS blocks with transistor-level op-amp designs.
- Add process-corner and Monte Carlo analysis for the analog circuits.
- Add layout implementation for the bandgap reference and phase shifter.
- Perform post-layout simulation after parasitic extraction.
- Improve project portability by adding setup notes for Cadence library import.

---

## Conclusion

These four projects together form a compact Analog IC and VLSI design portfolio. They demonstrate practical understanding of MOSFET behavior, analog signal processing, reference circuit design, and CMOS physical layout verification.

The work shows a complete learning progression from device-level simulation to circuit-level design and finally to layout-level verification.
