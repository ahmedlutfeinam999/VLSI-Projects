# Voltage-Controlled Phase Shifter using Cadence Virtuoso

<p align="center">
  <b>Schematic-Level Analog IC Design | Cadence Virtuoso | Spectre | gpdk090</b>
</p>

<p align="center">
  <img src="images/Final%20Circuit.png" alt="Final Phase Shifter Circuit" width="750">
</p>

---

## Project Summary

This project contains the schematic-level design and simulation of a **voltage-controlled positive phase shifter** using **Cadence Virtuoso**.

The circuit is based on a **first-order active all-pass phase shifter**. An **NMOS transistor from the gpdk090 PDK** is used as a voltage-controlled resistor. By changing the control voltage from **0 V to 1 V**, the phase shift changes from approximately **0° to 90° at 10 kHz**.

---

## Key Results

| Parameter | Target | Achieved |
|---|---:|---:|
| Frequency Range | 1 kHz – 100 kHz | Verified by AC sweep |
| Phase Shift Range | 0° – 90° | ~1.18° – 90.66° at 10 kHz |
| Control Voltage | 0 V – 1 V | Verified |
| Gain Response | ~0 dB | Close to 0 dB |
| 90° Time Shift at 10 kHz | 25 µs | Verified in transient simulation |

---

## Tools and Libraries Used

| Category | Tools / Libraries |
|---|---|
| Schematic Design | Cadence Virtuoso |
| Simulation | Cadence Spectre |
| Waveform Viewer | ViVA |
| PDK | gpdk090 |
| Simulation Library | analogLib |
| Environment | VMware Linux Environment |

---

## Main Cadence Libraries

| Library | Components Used |
|---|---|
| gpdk090 | nmos1v |
| analogLib | resistor, capacitor, vsource, vcvs, gnd |

---

## Final Circuit Information

| Component | Final Value / Device |
|---|---|
| R1 | 100 kΩ |
| R2 | 100 kΩ |
| C0 | 100 pF |
| Rseries | Tuned during simulation |
| Rbias | 100 MΩ |
| NMOS | gpdk090 nmos1v |
| NMOS Length | 1 µm |
| Amplifier Block | Ideal VCVS |
| VCVS Gain | 100 k |
| Control Voltage | 0 V – 1 V |

---

## Phase Shift at 10 kHz

| Vctrl | Phase Shift |
|---:|---:|
| 0 V | 1.18047° |
| 0.1 V | 19.13928° |
| 0.2 V | 68.0335° |
| 0.3 V | 83.528° |
| 0.4 V | 87.347° |
| 0.5 V | 88.825° |
| 0.6 V | 89.42781° |
| 0.7 V | 89.50859° |
| 0.8 V | 90.2587° |
| 0.9 V | 90.372° |
| 1 V | 90.657° |

---

## Repository Structure

```text
Voltage-Controlled-Phase-Shifter/
│
├── README.md
│
├── report/
│   └── Phase_Shifter_Project_Report.pdf
│
├── images/
│   ├── 90 degree phase shift at 10KHz.png
│   ├── ahdlLib opamp compile error.png
│   ├── Final Circuit.png
│   ├── First Circuit With Resistor instead of NMOS.png
│   ├── gain plot for final circuit at different Vctrl.png
│   ├── gain plot of the first resistor circuit instead of NMOS.png
│   ├── nmos primary -90 degree phase shift.png
│   ├── phase shift large.png
│   ├── phase shift.png
│   ├── Phasse Shift for different vctrl.png
│   ├── positive 90 phase shift.png
│   ├── vctrl 0 and zero phase shift.png
│   └── vctrl 1 and 90 phase shift.png
│
└── cadence_files/
    └── Phase_Shifter_Project/
        └── schematic/
```

---

## Folder Description

| Folder | Description |
|---|---|
| `report/` | Contains the final project report in PDF format. |
| `images/` | Contains schematic screenshots, AC plots, gain plots, transient plots, and error screenshots. |
| `cadence_files/` | Contains the direct Cadence project files. The files are not compressed into a `.tar.gz` archive. |

---

## Important Images

| Image | Description |
|---|---|
| `Final Circuit.png` | Final positive voltage-controlled phase shifter schematic. |
| `Phasse Shift for different vctrl.png` | AC phase response for different Vctrl values. |
| `gain plot for final circuit at different Vctrl.png` | Gain response of the final circuit. |
| `vctrl 0 and zero phase shift.png` | Transient result for Vctrl = 0 V. |
| `vctrl 1 and 90 phase shift.png` | Transient result for Vctrl = 1 V. |
| `ahdlLib opamp compile error.png` | Cadence AHDL compilation error encountered while trying to use ahdlLib opamp. |
| `First Circuit With Resistor instead of NMOS.png` | Initial fixed-resistor phase shifter circuit. |
| `nmos primary -90 degree phase shift.png` | Initial NMOS-based negative phase shift result. |
| `positive 90 phase shift.png` | Positive phase shift verification result. |

---

## How to Open the Cadence Project on Another PC

### Requirements

| Requirement | Needed |
|---|---|
| Cadence Virtuoso | Yes |
| Cadence Spectre | Yes |
| gpdk090 PDK | Yes |
| analogLib | Yes |
| Linux Environment | Yes |

---

### Step 1: Copy the Cadence Project Folder

Copy this folder from the repository:

```text
cadence_files/Phase_Shifter_Project
```

into a Cadence library folder.

Example:

```bash
mkdir -p ~/cadence/Anik
cp -r Phase_Shifter_Project ~/cadence/Anik/
```

After copying, the project should be located at:

```text
~/cadence/Anik/Phase_Shifter_Project
```

---

### Step 2: Add the Library to `cds.lib`

Open `cds.lib`:

```bash
gedit cds.lib
```

or:

```bash
nano cds.lib
```

Add this line:

```text
DEFINE Anik /home/USERNAME/cadence/Anik
```

Replace `USERNAME` with the Linux username.

Example:

```text
DEFINE Anik /home/student/cadence/Anik
```

The original library name used in this project was:

```text
Anik
```

So using the same library name is recommended.

---

### Step 3: Start Cadence Virtuoso

```bash
virtuoso &
```

---

### Step 4: Open the Design

In Cadence Library Manager:

| Field | Value |
|---|---|
| Library | Anik |
| Cell | Phase_Shifter_Project |
| View | schematic |

---

### Step 5: Add the gpdk090 Model Library

In ADE:

```text
Setup → Model Libraries
```

Add the gpdk090 Spectre model file, for example:

```text
gpdk090.scs
```

Use the nominal section used during this project:

```text
NN
```

---

## Simulation Setup

### AC Simulation

| Setting | Value |
|---|---|
| Input Source AC Magnitude | 1 |
| Input Source AC Phase | 0 |
| Control Voltage Variable | vctrl |
| Vctrl Sweep | 0 V to 1 V |
| Frequency Sweep | 1 kHz to 100 kHz |
| Sweep Type | Logarithmic |

Phase expression:

```text
phase(VF("/Vout") / VF("/Vin"))
```

---

### Transient Simulation

| Setting | Value |
|---|---|
| Input Type | Sine |
| Frequency | 10 kHz |
| Amplitude | 20 mV |
| Stop Time | 500 µs |

Expected transient behavior:

| Vctrl | Expected Result |
|---:|---|
| 0 V | Vin and Vout almost overlap |
| 1 V | Vout leads Vin by about 25 µs |

At 10 kHz:

```text
T = 1 / 10 kHz = 100 µs
90° phase shift = T / 4 = 25 µs
```

---

## Notes and Limitations

- The gpdk090 PDK is **not included** in this repository because PDK files may have licensing restrictions.
- The final circuit uses an **ideal VCVS** as the amplifier block.
- A supply-powered `ahdlLib` opamp was attempted, but it could not be used because of an AHDL compilation error in the Cadence environment.
- Therefore, the main phase-shifting behavior was verified using the ideal VCVS model.
- The Cadence project files are included directly as a folder, not as a compressed archive.

---

## Final Verification Summary

| Verification | Status |
|---|---|
| Schematic Design | Completed |
| AC Phase Sweep | Completed |
| Vctrl Parametric Sweep | Completed |
| Gain Plot | Completed |
| Transient Simulation | Completed |
| Positive 0°–90° Phase Shift | Verified |
| Full Supply-Powered Opamp Verification | Not completed due to AHDL compile error |

---

## Project Conclusion

This project demonstrates the schematic-level design and simulation of a **voltage-controlled positive phase shifter** using Cadence Virtuoso and gpdk090. The circuit successfully produces approximately **0° to 90° positive phase shift at 10 kHz** using a **0 V to 1 V control voltage**, while maintaining gain close to **0 dB**.
