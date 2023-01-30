# Advanced-Physical-Design-using-OpenLANE-Sky130
This repository contains the details of steps followed and summary of hands on done on doing the Advanced Physical Design Using OpenLANE/SKY130 workshop. The workshop focuses on complete ASIC Design flow from RTL2GDS using Open Source EDA tool OpenLANE and Google SKYWater130nm pdk. The core of design PICORV32A used is of RISC-V architechture.
<hr>

<br /> Table of Contents  
<br /> •	Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK
<br /> •	Day 2: Good Floorplan vs bad Floorplan and Introduction to Library Cells
<br /> •	Day 3 - Design library cell using Magic Layout and ngspice characterization
<br /> •	Day 4 - Pre-layout timing analysis and importance of good clock tree
<br /> •	Day 5 – Final Steps for RTL2GDS using TritonRoute and OpenSTA
<br />    References
<br />    Acknowledgement
<br />    Inquiries
<hr>

<br /> •	Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK

o	Talk to Computers 

The below figure shows the Block diagram of a System on a Chip.
![ ](Images/1.png) 

•	Package contains the semiconductor device. These packages protect the device from damage. They are of various types. An example of QFN-48 (Quad Falt No-Leads) with 48 pins is shown here.
![ ](Images/2.png) 
<br /> QFN stands for quad flat no-lead package. It is a leadless package that comes in small size and offers moderate heat dissipation in PCBs. Like any other IC package, the function of a QFN package is to connect the silicon die of the IC to the circuit board.
<br />•	Chip - It sits in the center of the package. The chip is connected to the package pins using wire bond. Inside the chip we have various components such as pad, core, interconnects, etc.
<br />•	Pads - These are the intermediate structure through which the internal signals from the core of IC is connected to the external pins of the chip. These pads are organized as Pad Frame. There are different kind of pads for input, output, power supply and ground.
<br />
![ ](Images/3.png) 
<br />•	Core - It is the place where all the logic units (gates, muxs, etc) are presnet inside the chip. These are able to execute the set of instructions given to the chip and produce an output.
<br />•	Die - It is the block which consists of semiconducting material and it can be used to build certain functional cuircuit which can be further sent for fabrication. It is the entire size of the
![ ](Images/4.png)


<br /> •	Day 2: Good Floorplan vs bad Floorplan and Introduction to Library Cells
<br /> •	Day 3 - Design library cell using Magic Layout and ngspice characterization
<br /> •	Day 4 - Pre-layout timing analysis and importance of good clock tree
<br /> •	Day 5 – Final Steps for RTL2GDS using TritonRoute and OpenSTA
