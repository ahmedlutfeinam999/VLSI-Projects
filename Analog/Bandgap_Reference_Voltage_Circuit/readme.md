# Bandgap Reference Project using Cadence Virtuoso

## Project Overview

This project contains the schematic-level design and simulation of a bandgap reference voltage circuit using Cadence Virtuoso.

The circuit was designed using a PMOS current mirror, PNP bipolar transistors, resistors, and ideal VCVS-based op-amp blocks. The bandgap core uses PTAT and CTAT voltage compensation to reduce temperature variation. The final output was scaled to meet the required reference voltage range.

The target project specifications were:

* Reference voltage, VREF: 1.4 V to 1.5 V
* Temperature coefficient: 10 ppm/°C to 17 ppm/°C
* Temperature range: -40°C to 125°C

The final simulated output voltage was approximately 1.45 V, and the temperature coefficient was approximately 14 ppm/°C over the required temperature range.

## Tools Used

* Cadence Virtuoso
* Cadence Spectre
* gpdk090 PDK
* analogLib
* ViVA Waveform Viewer
* VMware Linux Environment

## Main Libraries Used

* gpdk090
  * pmos1v
  * vpnp2

* analogLib
  * resistor
  * vsource
  * vcvs
  * gnd

## Design Method Summary

The reference voltage is based on the bandgap equation:

```text
VREF = |VBE3| + (R2/R1)VT ln(n)
```

Here, VBE3 is the CTAT part, which decreases with temperature. The term VT ln(n) is the PTAT part, which increases with temperature. By selecting the correct resistor ratio R2/R1 and BJT multiplier ratio n, the positive and negative temperature effects can nearly cancel each other.

In the final design, the bandgap core first generated a low-temperature-coefficient voltage of approximately 1.17 V. Then, an ideal VCVS-based non-inverting scaling stage was used to scale the voltage to approximately 1.45 V.

## Final Design Values

| Parameter | Final Value |
|---|---:|
| Supply voltage, VDC | 3.3 V |
| R1 | 1 kΩ |
| R2 | 6.85 kΩ |
| Left PNP multiplier | 1 |
| Middle PNP multiplier | 10 |
| Right/output PNP multiplier | 1 |
| PMOS width | 2.4 µm |
| PMOS length | 500 nm |
| VCVS gain | 100000 |
| Feedback resistor, Rg | 100 kΩ |
| Feedback resistor, Rf | 24 kΩ |
| Final output voltage | approximately 1.45 V |
| Temperature coefficient | approximately 14 ppm/°C |

## Project Result Summary

Over the temperature range of -40°C to 125°C:

* Final VREF remained within the required 1.4 V to 1.5 V range.
* Final VREF varied approximately between 1.448 V and 1.452 V.
* The total voltage variation was only a few millivolts.
* The calculated temperature coefficient was approximately 14 ppm/°C.
* The design satisfied the required temperature coefficient specification of 10 ppm/°C to 17 ppm/°C.

## Repository Folder Contents

```text
Bandgap-Reference-Project

* README.md

* report
  * Bandgap_Reference_Project_Report.docx

* images
  * final circuit schematic.png
  * VCVS internal structure.png
  * voltage graph in desired temperature range.png

* cadence_files
  * Bandgap_Reference_Project
```

## Description of Folders

### report

This folder contains the final project report.

File included:

```text
Bandgap_Reference_Project_Report.docx
```

The report explains the project theory, VREF equation, component selection, theoretical calculation, Cadence implementation, simulation result, and final verification.

### images

This folder contains the screenshots used for project documentation.

#### final circuit schematic.png

Shows the final Cadence schematic of the bandgap reference circuit, including the PMOS current mirror, PNP BJTs, resistors, VCVS ideal op-amp block, and output scaling stage.

#### VCVS internal structure.png

Shows the internal structure of the ideal VCVS-based op-amp block used in the project.

#### voltage graph in desired temperature range.png

Shows the final VREF versus temperature plot from -40°C to 125°C. The graph verifies that the output voltage remains within the required 1.4 V to 1.5 V range and satisfies the target temperature coefficient.

### cadence_files

This folder contains the direct Cadence project files.

Folder included:

```text
Bandgap_Reference_Project
```

The Cadence files are included directly as a project folder. They are not compressed as a tar file.

## How to Open the Cadence Files on Another PC

To open this project on another computer, the user needs:

* Cadence Virtuoso
* Cadence Spectre
* gpdk090 PDK installed
* analogLib available
* Linux environment

### Step 1: Copy the Cadence Project Folder

Copy this folder:

```text
cadence_files/Bandgap_Reference_Project
```

into a Cadence library directory.

Example:

```bash
mkdir -p ~/cadence/Bandgap_Project
cp -r Bandgap_Reference_Project ~/cadence/Bandgap_Project/
```

After copying, the project should be available as:

```text
~/cadence/Bandgap_Project/Bandgap_Reference_Project
```

### Step 2: Add the Library to cds.lib

Open the cds.lib file using a text editor.

Example:

```bash
gedit cds.lib
```

or:

```bash
nano cds.lib
```

Add this line:

```text
DEFINE Bandgap_Reference_Project /home/USERNAME/cadence/Bandgap_Project/Bandgap_Reference_Project
```

Replace USERNAME with the Linux username.

Example:

```text
DEFINE Bandgap_Reference_Project /home/student/cadence/Bandgap_Project/Bandgap_Reference_Project
```

### Step 3: Start Cadence Virtuoso

Run:

```bash
virtuoso &
```

### Step 4: Open the Design

In Cadence Library Manager, open the copied library:

```text
Library: Bandgap_Reference_Project
View: schematic
```

Open the main schematic cell inside the library.

### Step 5: Add the Model Libraries

In ADE, go to:

```text
Setup -> Model Libraries
```

Add the required gpdk090 Spectre model files.

Typical files used:

```text
gpdk090_mos.scs
gpdk090_bipolar.scs
```

Use the proper nominal model sections available in the local Cadence setup.

Important: do not add the same model file more than once, because duplicate model libraries can cause Spectre errors.

### Step 6: Run DC Operating Point Simulation

Set the design variables and component values according to the final design.

Main values:

```text
VDC = 3.3 V
R1 = 1 kΩ
R2 = 6.85 kΩ
Rf = 24 kΩ
Rg = 100 kΩ
```

Run DC operating point simulation at 27°C.

Expected result:

```text
Final VREF ≈ 1.45 V
```

### Step 7: Run Temperature Sweep

For temperature sweep:

```text
Start temperature = -40°C
Stop temperature = 125°C
Step = 5°C or 10°C
```

Plot the final reference output node:

```text
VREF or Vout
```

Expected result:

```text
VREF stays approximately between 1.448 V and 1.452 V
```

### Step 8: Calculate Temperature Coefficient

Use:

```text
TC = [(Vmax - Vmin) / (Vnominal × ΔT)] × 10^6
```

where:

```text
ΔT = 125 - (-40) = 165°C
```

For this project, the simulated TC was approximately:

```text
14 ppm/°C
```

## Important Notes

The gpdk090 PDK is not included in this repository because PDK files may have licensing restrictions.

Only the user-created Cadence project files, report, screenshots, and documentation are included.

If gpdk090 is not installed on another PC, the PMOS and PNP devices may not load correctly.

The final design is a schematic-level design. It uses ideal VCVS-based op-amp blocks for the feedback and output scaling stages. A full transistor-level op-amp and startup circuit were not implemented in this version.

The final output curve is slightly parabolic over temperature. This is normal for a first-order bandgap reference. The important point is that the total voltage variation is very small and the calculated temperature coefficient satisfies the project requirement.

## Final Verification Summary

* Target VREF range: 1.4 V to 1.5 V
* Simulated final VREF: approximately 1.45 V
* Target temperature range: -40°C to 125°C
* Target TC: 10 ppm/°C to 17 ppm/°C
* Simulated TC: approximately 14 ppm/°C
* Simulation type: DC operating point and temperature sweep
* Tool: Cadence Virtuoso ADE/Spectre

This project demonstrates the schematic-level design and simulation of a bandgap reference voltage circuit using Cadence Virtuoso and gpdk090.
