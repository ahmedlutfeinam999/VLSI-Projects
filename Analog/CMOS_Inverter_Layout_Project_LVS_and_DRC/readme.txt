CMOS Inverter Layout using Cadence Virtuoso

Project Overview

This project contains the custom layout design of a CMOS inverter using Cadence Virtuoso. The inverter was designed using one PMOS transistor and one NMOS transistor from the gpdk090 1V device library.

The complete design flow was done in this project:

* Schematic design
* Layout design
* DRC verification
* LVS verification
* Transient simulation
* Project documentation

The final layout passed both DRC and LVS.

Tools Used

* Cadence Virtuoso
* gpdk090 PDK
* Assura DRC
* Assura LVS
* ViVA Waveform Viewer
* VMware Linux Environment

Project Result

DRC: Passed with zero errors
LVS: Schematic and layout matched
Simulation: CMOS inverter operation verified

Repository Folder Contents

CMOS_Inverter_Layout_Project

* README.txt

* report

  * CMOS_Inverter_Layout_Project_Report.docx

* images

  * schematic.png
  * layout.png
  * no drc.png
  * LVS Summary.png

* cadence_files

  * CMOS_inverter_cadence_files.tar.gz

Description of Folders

report

This folder contains the editable project report.

File included:
CMOS_Inverter_Layout_Project_Report.pdf

The report explains the complete project flow, including schematic creation, layout design, body contacts, DRC, LVS, simulation result, and the issues faced during the project.

images

This folder contains the screenshots used for documentation.

schematic.png

This image shows the CMOS inverter schematic.

layout.png

This image shows the final CMOS inverter layout.

no drc.png

This image shows that the layout passed DRC with zero errors.

LVS Summary.png

This image shows that the schematic and layout matched successfully in LVS.

cadence_files

This folder contains the compressed Cadence design files.

File included:
CMOS_inverter_cadence_files.tar.gz

This file contains the Cadence project files for the CMOS inverter design.

How to Open the Cadence Files

The Cadence files are stored inside:

cadence_files/CMOS_inverter_cadence_files.tar.gz

To open these files, another user needs:

* Cadence Virtuoso
* gpdk090 PDK
* Assura DRC/LVS setup
* Linux environment

The gpdk090 PDK is required because this project uses the following devices:

* nmos1v
* pmos1v

Step-by-Step Guide to Open in Cadence

Step 1: Download the Cadence File

Download this file from the repository:

CMOS_inverter_cadence_files.tar.gz

Step 2: Copy It to a Cadence Working Directory

Example commands:

mkdir -p ~/cadence
cp CMOS_inverter_cadence_files.tar.gz ~/cadence/
cd ~/cadence

Step 3: Extract the File

Run this command:

tar -xzf CMOS_inverter_cadence_files.tar.gz

Depending on how the archive extracts, it may create a folder like:

CMOS_inverter

or:

home/buet/CMOS_inverter

If it extracts as home/buet/CMOS_inverter, move it to the current Cadence folder using:

mv home/buet/CMOS_inverter ./CMOS_inverter

After this, the folder should be available as:

~/cadence/CMOS_inverter

Step 4: Add the Library to cds.lib

Open the cds.lib file using any text editor.

Example:

gedit cds.lib

or:

nano cds.lib

Add this line:

DEFINE Anik /home/USERNAME/cadence/CMOS_inverter

Replace USERNAME with your Linux username.

Example:

DEFINE Anik /home/student/cadence/CMOS_inverter

The original Cadence library name used in this project was:

Anik

So the library should be defined as Anik.

Step 5: Start Cadence Virtuoso

Run this command:

virtuoso &

Step 6: Open the Design

In Cadence Library Manager, open:

Library: Anik
Cell: CMOS_inverter
View: schematic

or:

Library: Anik
Cell: CMOS_inverter
View: layout

Important Note

The gpdk090 PDK is not included in this repository because PDK files may have licensing restrictions.

Only the user-created Cadence files, report, and screenshots are included.

If another user does not have gpdk090 installed, the design may not open properly in Cadence. However, the project can still be reviewed using the report and images.

Final Verification Summary

DRC: Passed with zero errors
LVS: Schematic and layout matched
Simulation: CMOS inverter behavior verified

This project demonstrates the basic full-custom IC layout flow from schematic design to verified layout.
