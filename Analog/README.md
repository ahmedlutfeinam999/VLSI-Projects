<div align="center">

# Analog & VLSI Circuit Design Projects

### Analog Switching • Low-Power CMOS Classification • Transistor Characterization • Analog Circuit Design • Reference Circuit Design • CMOS Layout Verification

![Cadence Virtuoso](https://img.shields.io/badge/Cadence-Virtuoso-blue?style=for-the-badge)
![LTspice](https://img.shields.io/badge/LTspice-Simulation-red?style=for-the-badge)
![gpdk090](https://img.shields.io/badge/PDK-gpdk090-green?style=for-the-badge)
![TSMC 180nm](https://img.shields.io/badge/Process-TSMC%20180nm-orange?style=for-the-badge)
![DRC LVS](https://img.shields.io/badge/Verification-DRC%20%7C%20LVS-purple?style=for-the-badge)
![RCX](https://img.shields.io/badge/Extraction-RCX-brightgreen?style=for-the-badge)

</div>

---

## Project Overview

This repository contains a collection of **six Analog IC and VLSI design projects**. The projects cover analog switching with CMOS transmission gates, low-power transistor-level classification, MOSFET characterization, analog signal processing, bandgap reference design, and custom CMOS layout verification.

The main objective of this repository is to document practical circuit-design work using industry-style EDA tools such as **Cadence Virtuoso, LTspice, gpdk090 PDK, TSMC 180nm models, Spectre/ADE, ViVA, and Assura DRC/LVS/RCX**.

---

## Quick Navigation

<div align="center">

[![01 DEMUX](https://img.shields.io/badge/01-1%3A8%20ANALOG%20DEMUX-00B8D9?style=for-the-badge)](#project-01-demux)
[![02 Classifier](https://img.shields.io/badge/02-LOW--POWER%20CMOS%20CLASSIFIER-7B2CBF?style=for-the-badge)](#project-02-classifier)
[![03 MOSFET](https://img.shields.io/badge/03-MOSFET%20CHARACTERIZATION-2E8B57?style=for-the-badge)](#project-03-mosfet)
[![04 Phase](https://img.shields.io/badge/04-PHASE%20SHIFTER-E63946?style=for-the-badge)](#project-04-phase)
[![05 Bandgap](https://img.shields.io/badge/05-BANDGAP%20REFERENCE-F4A261?style=for-the-badge)](#project-05-bandgap)
[![06 Layout](https://img.shields.io/badge/06-CMOS%20INVERTER%20LAYOUT-5A189A?style=for-the-badge)](#project-06-inverter)

</div>

---

## Project Dashboard

| No. | Project | Design Area | Tools / Technology | Main Outcome |
| --- | --- | --- | --- | --- |
| 01 | [**1:8 Analog Demultiplexer**](#project-01-demux) | Analog switching / hierarchical CMOS | Cadence Virtuoso, gpdk090, Spectre/ADE, ViVA | 7-cell transmission-gate tree; all 8 routes verified; 24.51 Ω worst-case ON resistance |
| 02 | [**Low-Power CMOS Multi-Sensor Threshold Classifier**](#project-02-classifier) | Mixed-signal / VLSI decision circuit | Cadence Virtuoso, gpdk090, Assura DRC/LVS/RCX | Weighted CMOS classifier completed with DRC, LVS, RCX and 105.4 µW average transient power |
| 03 | [**MOSFET Characterization Suite**](#project-03-mosfet) | Device characterization | LTspice 26, TSMC 180nm BSIM3 | NMOS/PMOS Id, gm, gm/Id and small-signal behavior analyzed |
| 04 | [**Voltage-Controlled Phase Shifter**](#project-04-phase) | Analog signal processing | Cadence Virtuoso, gpdk090 nmos1v | Positive phase shift from about 1.18° to 90.66° at 10 kHz |
| 05 | [**Bandgap Reference Circuit**](#project-05-bandgap) | Analog reference design | Cadence Virtuoso, gpdk090 | 1.45 V reference with about 14.2 ppm/°C temperature coefficient |
| 06 | [**CMOS Inverter Layout**](#project-06-inverter) | Physical layout verification | Cadence Virtuoso Layout Suite, gpdk090, Assura | Custom layout passed DRC and LVS verification |

---

<a id="project-01-demux"></a>

# 01. 1:8 Analog Demultiplexer

![Cadence](https://img.shields.io/badge/Tool-Cadence%20Virtuoso-blue)
![PDK](https://img.shields.io/badge/PDK-gpdk090-green)
![Architecture](https://img.shields.io/badge/Architecture-CMOS%20Transmission%20Gate-00B8D9)
![Routing](https://img.shields.io/badge/Routing-8%2F8%20Verified-brightgreen)
![RON](https://img.shields.io/badge/Worst--Case%20RON-24.51%20Ohm-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)

## Summary

This project implements a **1:8 analog demultiplexer** in Cadence Virtuoso using a hierarchical CMOS transmission-gate architecture. The complete DEMUX is formed by cascading **seven 1:2 DEMUX cells** in a three-stage `1 → 2 → 4` binary tree. Each 1:2 cell contains two CMOS transmission gates, and each transmission gate uses one NMOS and one PMOS in parallel.

Three select inputs, `S2`, `S1`, and `S0`, route one analog input `VIN` to exactly one of eight outputs `Y0`–`Y7`. External `0 V / 2 V` logic is behaviorally converted to complementary `±4 V` transmission-gate controls.

## Architecture

<div align="center">
  <img src="./assets/demux_1to8_architecture.svg" alt="Hierarchical 1 to 8 analog demultiplexer architecture" width="950">
</div>

The diagram above shows the full **1 → 2 → 4 → 8** hierarchy. `I0` is controlled by `S2`, `I1` and `I2` are controlled by `S1`, and `I3` to `I6` are controlled by `S0`. This is why cascading **seven 1:2 DEMUX cells** produces the final **1:8 analog DEMUX**.

- **7 × 1:2 DEMUX cells**
- **14 transmission gates**
- **28 MOSFETs total**
- **3 transmission gates in every selected analog path**

## Main Verification Results

| Criterion | Requirement | Achieved |
| --- | ---: | ---: |
| Supply voltage | ±4 V | ±4 V |
| Logic HIGH / LOW | 2 V / 0 V | 2 V / 0 V |
| Analog signal range | -3.5 V to +3.5 V | Full range verified |
| ON resistance | 10–25 Ω | **24.51 Ω worst case** |
| Input capacitance | 50–65 pF | **55.07 pF** |
| Turn-ON time | ≤120 ns @ 1 kΩ ∥ 10 pF | **0.591 ns** |
| Charge injection | ≤20 pC | **0.287 pC worst case** |
| Simulated core/rail power | ≤25 mW | **≈1.80 µW*** |
| -3 dB bandwidth | >1.2 MHz accepted fallback | **>1 GHz** |
| Functional routing | Correct Y0–Y7 selection | **All 8 codes verified** |

\* The measured power is the schematic-level transmission-gate/core rail power with ideal VCVS control conversion. A practical transistor-level level shifter would increase total power.

## Main Learning Outcome

This project demonstrates hierarchical analog design using CMOS transmission gates and shows how **ON resistance, capacitive loading, switching time, charge injection, bandwidth, and output loading** interact in a practical analog switching network. Separate DC, AC, and transient simulations were used to verify each electrical requirement rather than relying on a single testbench.

[⬆ Back to Project Dashboard](#project-dashboard)

---
<a id="project-02-classifier"></a>

# 02. Low-Power CMOS Multi-Sensor Threshold Classifier

![Tool](https://img.shields.io/badge/Tool-Cadence%20Virtuoso-blue)
![PDK](https://img.shields.io/badge/PDK-GPDK%2090nm-lightgrey)
![DRC](https://img.shields.io/badge/DRC-Passed-brightgreen)
![LVS](https://img.shields.io/badge/LVS-Matched-brightgreen)
![RCX](https://img.shields.io/badge/RCX-Successful-brightgreen)
![Status](https://img.shields.io/badge/Status-Completed-success)
![Avg Power](https://img.shields.io/badge/Avg%20Power-105.4%20uW-orange)

## Summary

This project implements a low-power CMOS multi-sensor threshold classifier in Cadence Virtuoso using the gpdk090 PDK. The circuit accepts three analog sensor-like input voltages and produces a digital `alarm` output when the weighted input condition crosses the switching threshold.

The project demonstrates a complete VLSI design and verification flow: schematic design, symbol/testbench creation, DC sweep simulation, transient simulation, manual layout, DRC correction, LVS matching, RCX/parasitic extraction, extracted-view generation, and power analysis.

## Circuit Concept

The classifier behaves like a fixed weighted decision circuit:

```text
Decision strength ≈ w1·Vgas + w2·Vhum + w3·Vtemp
```

The weights are not stored digitally. They are implemented physically by using different NMOS transistor widths. A wider NMOS has stronger pull-down capability for the same gate voltage, so it has a stronger influence on the internal decision node.

## Transistor-Level Design

| Transistor | Type | Function | Width | Length |
| --- | --- | --- | ---: | ---: |
| MLOAD | PMOS | Pull-up load for internal decision node | 500 nm | 100 nm |
| MGAS | NMOS | Pull-down device controlled by `Vgas` | 4 µm | 100 nm |
| MHUM | NMOS | Pull-down device controlled by `Vhum` | 2 µm | 100 nm |
| MTEMP | NMOS | Pull-down device controlled by `Vtemp` | 1 µm | 100 nm |
| MPINV | PMOS | PMOS of output inverter | 2 µm | 100 nm |
| MNINV | NMOS | NMOS of output inverter | 1 µm | 100 nm |

The input influence ratio is approximately:

```text
MGAS : MHUM : MTEMP = 4 : 2 : 1
```

This means `Vgas` has the strongest influence, `Vhum` has medium influence, and `Vtemp` has the weakest influence.

## Working Principle

The circuit has two main parts:

1. **Weighted NMOS pull-down network**  
   `MGAS`, `MHUM`, and `MTEMP` pull the internal node downward depending on the input voltages and transistor widths.

2. **CMOS inverter output stage**  
   The internal decision node drives a CMOS inverter. When the internal node is high, `alarm` stays low. When the weighted pull-down network pulls the internal node low enough, the inverter output switches high.

| Internal decision node | Alarm output |
| --- | --- |
| High | Low |
| Low | High |

## Simulation Results

DC sweep simulations confirmed that the switching threshold depends on the input transistor width.

| Swept input | Relative influence | Explanation |
| --- | --- | --- |
| `Vgas` | Strongest | Controlled by the widest NMOS |
| `Vhum` | Medium | Controlled by medium-width NMOS |
| `Vtemp` | Weakest | Controlled by the narrowest NMOS |

Transient simulation confirmed that the alarm turns ON when the input condition rises above the effective threshold and turns OFF when the input returns to the safe region.

## Power Analysis

Average transient power was calculated from the VDD source current.

Measured average current:

```text
Iavg = -105.4 µA
```

The negative sign is due to the Cadence voltage-source current convention. Taking the magnitude:

```text
|Iavg| = 105.4 µA
```

Since:

```text
VDD = 1 V
```

the average transient power is:

```text
Pavg = VDD × |Iavg| = 1 V × 105.4 µA = 105.4 µW
```

| Power metric | Approximate value | Meaning |
| --- | ---: | --- |
| Idle/safe-state power | 5–10 µW | Low-input condition |
| Alarm-state static power | 145–150 µW | Alarm ON condition |
| Peak switching power | ~230 µW | Switching spike |
| Average transient power | 105.4 µW | Average over the simulated 0–8 ms input condition |

## Layout and Verification

The layout was created manually in Cadence Virtuoso. PMOS devices were placed near the VDD rail, NMOS devices were placed near the GND rail, and routing was performed using metal layers. Proper body connections were added for both PMOS and NMOS devices.

The first DRC run failed with three errors related to spacing/width rules. The errors were corrected and the final DRC passed successfully.

| Verification step | Result |
| --- | --- |
| First DRC | Failed with 3 errors |
| Final DRC | Passed with 0 errors |
| LVS | Schematic and layout matched |
| RCX | Successful |
| Extracted view | `av_extracted` generated |

## Main Learning Outcome

This project shows how a multi-input weighted decision can be implemented using only transistor-level CMOS sizing and threshold behavior. It also strengthens the complete physical design flow: schematic, layout, DRC, LVS, RCX, and power analysis.

[⬆ Back to Project Dashboard](#project-dashboard)

---
<a id="project-03-mosfet"></a>

# 03. MOSFET Characterization Suite

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

[⬆ Back to Project Dashboard](#project-dashboard)

---
<a id="project-04-phase"></a>

# 04. Voltage-Controlled Phase Shifter

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

[⬆ Back to Project Dashboard](#project-dashboard)

---
<a id="project-05-bandgap"></a>

# 05. Bandgap Reference Circuit

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

[⬆ Back to Project Dashboard](#project-dashboard)

---
<a id="project-06-inverter"></a>

# 06. CMOS Inverter Layout

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

[⬆ Back to Project Dashboard](#project-dashboard)

---

## Skills Demonstrated

| Skill Area | Demonstrated Through |
| --- | --- |
| Analog switching | 1:8 CMOS transmission-gate DEMUX, hierarchical routing, ON-resistance, bandwidth and charge-injection verification |
| Low-power classification | Weighted transistor-level multi-sensor threshold classifier with post-layout verification |
| MOSFET characterization | Id-Vgs, Id-Vds, gm, gm/Id and operating-point analysis |
| Analog circuit design | Phase shifter and bandgap reference design |
| Biasing and sizing | NMOS resistor tuning, PMOS mirror sizing, resistor-ratio tuning |
| Temperature analysis | Bandgap reference simulation from -40°C to 125°C |
| Physical layout | CMOS inverter and threshold-classifier custom layout in Cadence Virtuoso |
| Verification | Assura DRC, LVS and RCX checks |
| Power analysis | VDD-current based average and peak power estimation |
| EDA tools | LTspice, Cadence Virtuoso, ADE, ViVA, Assura |

---

## Repository Structure

```text
.
├── 1_to_8_Analog_Demultiplexer/
│   ├── cadence_files/
│   ├── images/
│   └── report/
│
├── Low_Power_CMOS_Multi_Sensor_Threshold_Classifier/
│   ├── cadence_files/
│   │   └── ai_classifier_v2.zip
│   ├── images/
│   └── report/
│
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
4. For Cadence projects provided as ZIP files, unzip the file first and copy the extracted project/cell folder into a compatible Cadence library.
5. Make sure the required technology files or PDKs are available before running the simulations.

### Tool Requirements

| Project | Required Tool / Setup |
| --- | --- |
| 1:8 Analog Demultiplexer | Cadence Virtuoso with gpdk090, Spectre/ADE and ViVA |
| Low-Power CMOS Multi-Sensor Threshold Classifier | Cadence Virtuoso with gpdk090, Spectre/ADE, ViVA, Assura DRC/LVS/RCX |
| MOSFET Characterization Suite | LTspice 26 with TSMC 180nm BSIM3 model files |
| Voltage-Controlled Phase Shifter | Cadence Virtuoso with gpdk090 |
| Bandgap Reference Circuit | Cadence Virtuoso with gpdk090 |
| CMOS Inverter Layout | Cadence Virtuoso Layout Suite with gpdk090 and Assura |

---

## Future Improvements

- Replace the DEMUX ideal VCVS control conversion with a transistor-level CMOS level shifter and voltage-rated devices.
- Add layout, DRC/LVS/RCX, PVT and post-layout verification for the 1:8 analog DEMUX.
- Replace ideal VCVS blocks in the other analog circuits with transistor-level op-amp designs.
- Add process-corner and Monte Carlo analysis for the analog circuits.
- Add layout implementation for the bandgap reference and phase shifter.
- Perform post-layout simulation after parasitic extraction.
- Optimize the threshold-classifier design for lower static power.
- Add PVT and extracted-view simulation for all layout-level projects.
- Improve project portability by adding setup notes for Cadence library import.

---

## Conclusion

These six projects together form a compact Analog IC and VLSI design portfolio. They demonstrate practical understanding of CMOS analog switching, low-power transistor-level classification, MOSFET behavior, analog signal processing, reference circuit design, and CMOS physical layout verification.

The work shows a learning progression from device-level characterization and schematic-level analog design to hierarchical switching networks and layout-level verification, including DRC, LVS, RCX extraction, timing behavior, and power analysis.
