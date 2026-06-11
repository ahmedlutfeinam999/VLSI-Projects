Voltage-Controlled Phase Shifter using Cadence Virtuoso

Project Overview

This project contains the schematic-level design and simulation of a voltage-controlled positive phase shifter using Cadence Virtuoso.

The circuit was designed as an active all-pass phase shifter. An NMOS transistor from the gpdk090 PDK was used as a voltage-controlled resistor. By changing the control voltage from 0 V to 1 V, the phase shift changes from approximately 0 degree to 90 degree at 10 kHz.

The project was verified using AC phase analysis, gain analysis, and transient simulation.

Tools Used

* Cadence Virtuoso
* Cadence Spectre
* gpdk090 PDK
* analogLib
* ViVA Waveform Viewer
* VMware Linux Environment

Main Libraries Used

* gpdk090

  * nmos1v

* analogLib

  * resistor
  * capacitor
  * vsource
  * vcvs
  * gnd

Project Result Summary

At 10 kHz:

* Vctrl = 0 V produced approximately 1.18 degree phase shift
* Vctrl = 1 V produced approximately 90.66 degree phase shift
* Gain remained close to 0 dB
* Transient simulation showed nearly overlapping Vin and Vout for Vctrl = 0 V
* Transient simulation showed approximately 25 us time shift for Vctrl = 1 V

Repository Folder Contents

Voltage-Controlled-Phase-Shifter

* README.txt

* report

  * Phase_Shifter_Project_Report.pdf

* images

  * 90 degree phase shift at 10KHz.png
  * ahdlLib opamp compile error.png
  * Final Circuit.png
  * First Circuit With Resistor instead of NMOS.png
  * gain plot for final circuit at different Vctrl.png
  * gain plot of the first resistor circuit instead of NMOS.png
  * nmos primary -90 degree phase shift.png
  * phase shift large.png
  * phase shift.png
  * Phasse Shift for different vctrl.png
  * positive 90 phase shift.png
  * vctrl 0 and zero phase shift.png
  * vctrl 1 and 90 phase shift.png

* cadence_files

  * Phase_Shifter_Project

Description of Folders

report

This folder contains the final project report.

File included:

Phase_Shifter_Project_Report.pdf

The report explains the theory, design process, calculations, component selection, simulation results, and issues faced during the project.

images

This folder contains the screenshots used for project documentation.

90 degree phase shift at 10KHz.png

Shows the 90 degree phase shift result at 10 kHz.

ahdlLib opamp compile error.png

Shows the Cadence AHDL compilation error encountered when trying to use the ahdlLib opamp.

Final Circuit.png

Shows the final voltage-controlled positive phase shifter schematic.

First Circuit With Resistor instead of NMOS.png

Shows the first fixed-resistor phase shifter circuit before replacing the resistor with an NMOS.

gain plot for final circuit at different Vctrl.png

Shows the gain response of the final circuit for different control voltage values.

gain plot of the first resistor circuit instead of NMOS.png

Shows the gain response of the initial fixed-resistor circuit.

nmos primary -90 degree phase shift.png

Shows the first NMOS-based phase shifter result that produced negative phase shift.

phase shift large.png

Shows an intermediate phase-shift result during tuning.

phase shift.png

Shows a phase response result from the design process.

Phasse Shift for different vctrl.png

Shows the phase response for different Vctrl values.

positive 90 phase shift.png

Shows the positive phase-shift result after modifying the circuit topology.

vctrl 0 and zero phase shift.png

Shows transient simulation for Vctrl = 0 V, where Vin and Vout are almost in phase.

vctrl 1 and 90 phase shift.png

Shows transient simulation for Vctrl = 1 V, where Vout leads Vin by approximately 90 degrees.

cadence_files

This folder contains the direct Cadence project files.

Folder included:

Phase_Shifter_Project

The Cadence files are included directly as a project folder. They are not compressed as a tar file.

How to Open the Cadence Files on Another PC

To open this project on another computer, the user needs:

* Cadence Virtuoso
* Cadence Spectre
* gpdk090 PDK installed
* analogLib available
* Linux environment

Step 1: Copy the Cadence Project Folder

Copy this folder:

cadence_files/Phase_Shifter_Project

into a Cadence library folder.

Example:

mkdir -p ~/cadence/Anik

cp -r Phase_Shifter_Project ~/cadence/Anik/

After copying, the project should be available as:

~/cadence/Anik/Phase_Shifter_Project

Step 2: Add the Library to cds.lib

Open the cds.lib file using a text editor.

Example:

gedit cds.lib

or:

nano cds.lib

Add this line:

DEFINE Anik /home/USERNAME/cadence/Anik

Replace USERNAME with the Linux username.

Example:

DEFINE Anik /home/student/cadence/Anik

The original library name used in this project was:

Anik

So it is recommended to use the same library name.

Step 3: Start Cadence Virtuoso

Run:

virtuoso &

Step 4: Open the Design

In Cadence Library Manager, open:

Library: Anik
Cell: Phase_Shifter_Project
View: schematic

Step 5: Add the Model Library

In ADE, go to:

Setup -> Model Libraries

Add the gpdk090 Spectre model file.

Example:

gpdk090.scs

Use the nominal model section used in the project, such as:

NN

Step 6: Run AC Simulation

For AC simulation:

* Set Vin source AC magnitude to 1
* Set Vctrl source DC value as vctrl
* Sweep vctrl from 0 V to 1 V
* Sweep frequency from 1 kHz to 100 kHz

Phase expression used:

phase(VF("/Vout") / VF("/Vin"))

Step 7: Run Transient Simulation

For transient simulation:

* Set Vin as sine input
* Frequency = 10 kHz
* Amplitude = 20 mV
* Stop time = 500 us

For Vctrl = 0 V:

Vin and Vout should almost overlap.

For Vctrl = 1 V:

Vout should lead Vin by about 25 us.

This 25 us time shift is expected because at 10 kHz the time period is 100 us. A 90 degree phase shift is one quarter of the period, so:

100 us / 4 = 25 us

Important Notes

The gpdk090 PDK is not included in this repository because PDK files may have licensing restrictions.

Only the user-created Cadence project files, report, screenshots, and documentation are included.

If gpdk090 is not installed on another PC, the NMOS device may not load correctly.

The final circuit uses an ideal VCVS as the amplifier block. A supply-powered opamp was attempted, but the ahdlLib opamp could not be used due to an AHDL compilation error in the Cadence setup.

Final Verification Summary

Phase shift range: approximately 0 degree to 90 degree at 10 kHz

Control voltage range: 0 V to 1 V

Frequency range tested: 1 kHz to 100 kHz

Gain response: close to 0 dB

Transient response: verified for both Vctrl = 0 V and Vctrl = 1 V

This project demonstrates the schematic-level design and simulation of a voltage-controlled positive phase shifter using Cadence Virtuoso and gpdk090.
