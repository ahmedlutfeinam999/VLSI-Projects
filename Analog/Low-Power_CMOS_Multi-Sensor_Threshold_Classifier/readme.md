# Low-Power CMOS Multi-Sensor Threshold Classifier

This repository contains the Cadence Virtuoso design files, simulation screenshots, layout verification results, and project report for a **low-power CMOS multi-sensor threshold classifier** developed as part of an **EEE 4861 assignment**.

The project demonstrates a complete transistor-level VLSI design flow: schematic design, symbol/testbench creation, DC and transient simulation, layout design, DRC correction, LVS verification, RCX/parasitic extraction, and power analysis.

---

## Project Overview

| Item | Description |
|---|---|
| Project type | Transistor-level CMOS/VLSI design |
| Main function | Multi-input weighted threshold classification |
| Inputs | `Vgas`, `Vhum`, `Vtemp` |
| Output | `alarm` |
| Technology | 90 nm CMOS using `gpdk090` |
| Supply voltage | 1 V |
| Design tool | Cadence Virtuoso |
| Simulator | Spectre / ADE |
| Verification tools | Assura DRC, Assura LVS, Assura RCX |
| Course context | EEE 4861 assignment |

---

## Repository Structure

```text
.
├── README.md
│
├── cadence_files/
│   ├── ai_classifier_v2.zip
│   └── readme.txt
│
├── images/
│   ├── 1st DRC run 3 errors.png
│   ├── ai classifer inside circuit white bg.png
│   ├── ai classifer inside circuit.png
│   ├── ai classifier test bench dc vgas.png
│   ├── av_extracted figure white bg.png
│   ├── av_extracted figure.png
│   ├── average current.png
│   ├── DC sweep of Vgas vs alarm output white bg.png
│   ├── DC sweep of Vhum vs alarm output white bg.png
│   ├── DC sweep of Vtemp vs alarm output white bg.png
│   ├── final layout.png
│   ├── layout white bg clr.png
│   ├── layout white bg.png
│   ├── LVS Details.png
│   ├── LVS Matched.png
│   ├── no DRC error.png
│   ├── RCX Successful.png
│   ├── transient response white bg.png
│   ├── Vdd current transient analysis white bg.png
│   └── Vpulse as Gas Source.png
│
└── report/
    └── Low-Power_CMOS_Multi-Sensor_Threshold_Classifier_Project_Report.pdf
```

The Cadence project is provided as a ZIP file because Cadence cell folders usually contain many internal files. Keeping the Cadence project as one ZIP file makes the GitHub repository cleaner and avoids upload issues.

---

## Main Circuit Idea

The circuit is a **weighted CMOS threshold classifier**. It accepts three analog input voltages and generates a digital alarm output.

The three input voltages represent sensor-like signals:

| Input | Role |
|---|---|
| `Vgas` | First sensor input |
| `Vhum` | Second sensor input |
| `Vtemp` | Third sensor input |

The output is:

| Output | Meaning |
|---|---|
| `alarm = 0` | Weighted input condition is below the switching threshold |
| `alarm = 1` | Weighted input condition crosses the switching threshold |

The circuit does not use software or a microcontroller. The decision is made directly by MOS transistor behavior.

---

## Weighted Threshold Concept

The circuit behaves like a fixed weighted decision system:

```text
Decision strength ≈ w1·Vgas + w2·Vhum + w3·Vtemp
```

In this design, the weights are implemented physically using NMOS transistor widths.

A wider NMOS transistor can conduct more current for the same input voltage. Therefore, it has a stronger ability to pull down the internal decision node.

---

## Transistor Sizing

| Transistor | Type | Function | Width | Length |
|---|---|---|---:|---:|
| `MLOAD` | PMOS | Pull-up load for internal decision node | 500 nm | 100 nm |
| `MGAS` | NMOS | Pull-down device controlled by `Vgas` | 4 µm | 100 nm |
| `MHUM` | NMOS | Pull-down device controlled by `Vhum` | 2 µm | 100 nm |
| `MTEMP` | NMOS | Pull-down device controlled by `Vtemp` | 1 µm | 100 nm |
| `MPINV` | PMOS | PMOS of output inverter | 2 µm | 100 nm |
| `MNINV` | NMOS | NMOS of output inverter | 1 µm | 100 nm |

The input weight ratio is approximately:

```text
MGAS : MHUM : MTEMP = 4 : 2 : 1
```

So `Vgas` has the strongest influence, `Vhum` has medium influence, and `Vtemp` has the weakest influence.

---

## How the Circuit Works

The circuit has two main parts:

### 1. Weighted NMOS Pull-Down Network

The input transistors `MGAS`, `MHUM`, and `MTEMP` form a parallel NMOS pull-down network. Their drains are connected to the internal decision node, and their sources are connected to ground.

When an input voltage increases, the corresponding NMOS turns on more strongly and pulls the internal node downward.

Because the NMOS widths are different, each input has a different strength.

### 2. Output Inverter

The internal decision node drives a CMOS inverter made from `MPINV` and `MNINV`.

| Internal node | Alarm output |
|---|---|
| High | Low |
| Low | High |

When the weighted NMOS network pulls the internal node low enough, the inverter output switches high and the alarm turns ON.

---

## Schematic

The schematic contains the PMOS load, weighted NMOS pull-down network, and CMOS inverter output stage.

![Circuit schematic](images/ai%20classifer%20inside%20circuit%20white%20bg.png)

---

## Testbench

The testbench provides the supply voltage and the three input sources.

![Testbench](images/ai%20classifier%20test%20bench%20dc%20vgas.png)

For DC sweep simulations, one input is swept from 0 V to 1 V while the other inputs are held at low values.

---

## DC Sweep Results

### Vgas Sweep

`Vgas` causes the earliest switching because `MGAS` has the largest width.

![DC sweep Vgas](images/DC%20sweep%20of%20Vgas%20vs%20alarm%20output%20white%20bg.png)

### Vhum Sweep

`Vhum` has medium influence because `MHUM` has medium width.

![DC sweep Vhum](images/DC%20sweep%20of%20Vhum%20vs%20alarm%20output%20white%20bg.png)

### Vtemp Sweep

`Vtemp` has the weakest influence because `MTEMP` has the smallest width.

![DC sweep Vtemp](images/DC%20sweep%20of%20Vtemp%20vs%20alarm%20output%20white%20bg.png)

### DC Sweep Interpretation

| Swept input | Relative influence | Reason |
|---|---|---|
| `Vgas` | Strongest | Widest NMOS transistor |
| `Vhum` | Medium | Medium-width NMOS transistor |
| `Vtemp` | Weakest | Narrowest NMOS transistor |

The DC sweep results confirm that transistor width can be used to control the relative influence of each input.

---

## Transient Simulation

A transient input was applied to observe time-domain alarm switching.

![Transient response](images/transient%20response%20white%20bg.png)

When the input rises above the effective threshold, the alarm output turns ON. When the input falls back down, the alarm output turns OFF again.

---

## Power Analysis

Power was calculated using the current through the VDD source.

The measured average current was:

```text
Iavg = -105.4 µA
```

The negative sign comes from the Cadence voltage source current direction convention. The actual consumed current magnitude is:

```text
|Iavg| = 105.4 µA
```

Since the supply voltage is:

```text
VDD = 1 V
```

the average transient power is:

```text
Pavg = VDD × |Iavg|
Pavg = 1 V × 105.4 µA
Pavg = 105.4 µW
```

![Average current calculation](images/average%20current.png)

![VDD current waveform](images/Vdd%20current%20transient%20analysis%20white%20bg.png)

### Power Summary

| Power metric | Approximate value | Meaning |
|---|---:|---|
| Idle/safe-state power | 5–10 µW | Low-input condition |
| Alarm-state static power | 145–150 µW | Alarm ON condition |
| Peak switching power | ~230 µW | Switching spike |
| Average transient power | 105.4 µW | Average over the simulated 0–8 ms input condition |

The average power is specific to the applied transient test condition. It should not be treated as a universal lifetime power value.

---

## Layout

The layout follows a standard CMOS arrangement:

- PMOS devices are placed close to the VDD rail.
- NMOS devices are placed close to the GND rail.
- Input pull-down transistors are arranged as the weighted decision network.
- The inverter stage is placed near the output routing.
- Body connections are tied properly.
- Routing is performed using metal layers.

![Layout color view](images/layout%20white%20bg%20clr.png)

![Layout black and white view](images/layout%20white%20bg.png)

---

## DRC Verification

The first DRC run reported three errors.

![First DRC errors](images/1st%20DRC%20run%203%20errors.png)

After fixing the layout spacing and width issues, DRC passed with no errors.

![No DRC error](images/no%20DRC%20error.png)

| DRC stage | Result |
|---|---|
| First DRC run | Failed with 3 errors |
| Final DRC run | Passed with 0 errors |

---

## LVS Verification

LVS was performed to verify that the layout matches the schematic.

![LVS matched](images/LVS%20Matched.png)

The detailed LVS summary shows no net, device, pin, or parameter mismatches.

![LVS details](images/LVS%20Details.png)

| LVS check | Result |
|---|---|
| Schematic vs layout | Matched |
| Net mismatches | 0 |
| Device mismatches | 0 |
| Pin mismatches | 0 |
| Parameter mismatches | 0 |

---

## RCX / Parasitic Extraction

After successful DRC and LVS, parasitic extraction was performed using Assura RCX.

![RCX successful](images/RCX%20Successful.png)

The extracted view generated by RCX is:

```text
av_extracted
```

![Extracted view](images/av_extracted%20figure%20white%20bg.png)

---

# How to Use the Cadence Project

The Cadence design is stored as a ZIP file:

```text
cadence_files/ai_classifier_v2.zip
```

You must unzip it first before using it in Cadence.

---

## Important Compatibility Requirements

This project is not guaranteed to work on every Cadence installation. It should work if the user has a compatible setup.

| Requirement | Details |
|---|---|
| Cadence Virtuoso | Same or compatible version recommended |
| PDK | `gpdk090` required |
| Devices | `gpdk090_nmos1v`, `gpdk090_pmos1v` |
| Simulator | Spectre recommended |
| Verification | Assura DRC/LVS/RCX used |
| Library setup | Correct `cds.lib` paths required |

If `gpdk090` is not installed or attached correctly, the schematic/layout may open with missing device symbols or unresolved instances.

---

## Method 1: Using File Manager

1. Download this repository.
2. Go to:

```text
cadence_files/
```

3. Extract:

```text
ai_classifier_v2.zip
```

After extraction, you should get a folder named:

```text
ai_classifier_v2
```

4. Copy the extracted folder:

```text
ai_classifier_v2
```

5. Paste it into your Cadence working library directory.

Example:

```text
/home/username/cadence/Inam/ai_classifier_v2
```

or any existing Cadence library directory that is attached to `gpdk090`.

6. Open Cadence Virtuoso.
7. Open Library Manager.
8. Refresh the library if needed.
9. Open the cell:

```text
ai_classifier_v2
```

10. Check for views such as:

```text
schematic
symbol
layout
av_extracted
```

---

## Method 2: Using Terminal

From the repository root:

```bash
cd cadence_files
unzip ai_classifier_v2.zip
```

This creates:

```text
ai_classifier_v2/
```

Copy it into your Cadence library folder:

```bash
cp -r ai_classifier_v2 /home/username/cadence/Inam/
```

Then start Cadence from the proper working directory:

```bash
virtuoso &
```

Open the cell from Library Manager.

---

## Method 3: Defining the Extracted Folder as a Library

If you want to keep the extracted folder as a separate Cadence library:

1. Extract the ZIP file.

```bash
cd cadence_files
unzip ai_classifier_v2.zip
```

2. Move it to your Cadence directory.

```bash
mv ai_classifier_v2 /home/username/cadence/
```

3. Open your `cds.lib` file.

```bash
gedit /home/username/cadence/cds.lib
```

4. Add this line:

```text
DEFINE ai_classifier_v2 /home/username/cadence/ai_classifier_v2
```

5. Start Cadence from the directory containing the edited `cds.lib`.

```bash
virtuoso &
```

6. Check whether `ai_classifier_v2` appears in Library Manager.

---

## Running the Project

### Schematic Simulation

1. Open the schematic/testbench view.
2. Launch ADE.
3. Select Spectre as simulator.
4. Set `VDD = 1 V`.
5. Run DC sweep or transient analysis.
6. Plot the desired nodes:

```text
Vgas
Vhum
Vtemp
alarm
/VDD/PLUS
```

### DRC

Open the layout view and run:

```text
Assura → Run DRC
```

Expected final result:

```text
No DRC errors found.
```

### LVS

Run:

```text
Assura → Run LVS
```

Expected final result:

```text
Schematic and Layout Match.
```

### RCX

Run:

```text
Assura → Run RCX
```

Expected extracted view:

```text
av_extracted
```

---

## Results Summary

| Task | Status |
|---|---|
| Schematic design | Completed |
| Testbench creation | Completed |
| DC sweep simulation | Completed |
| Transient simulation | Completed |
| Layout design | Completed |
| First DRC | Failed with 3 errors |
| Final DRC | Passed |
| LVS | Matched |
| RCX | Successful |
| Extracted view | Generated |
| Average transient power | 105.4 µW |

---

## Limitations

This is a course-level VLSI design and verification project. It is not a commercial tapeout-ready design.

Known limitations:

- No full PVT corner analysis
- No Monte Carlo analysis
- No final GDSII tapeout package
- No pad ring or ESD protection
- No silicon measurement
- Power is measured only for the simulated transient input pattern
- Requires compatible Cadence and `gpdk090` setup

---

## Possible Future Improvements

- Run post-layout simulation from the `av_extracted` view
- Perform PVT corner analysis
- Add Monte Carlo mismatch analysis
- Optimize sizing for lower static current
- Add programmable threshold control
- Add a latch for sticky alarm behavior
- Add complete sensor front-end circuits
- Add final GDSII export flow

---

## Conclusion

This project demonstrates a complete CMOS/VLSI design flow for a low-power multi-sensor threshold classifier. The key concept is that transistor width can be used to control input influence. Wider NMOS devices create stronger pull-down paths, giving larger weight to the corresponding input.

The final design successfully passed DRC and LVS, and RCX generated the extracted view. The average transient power measured over the simulated 0–8 ms input condition was approximately:

```text
105.4 µW
```
