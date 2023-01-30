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
<br />•	Die - It is the block which consists of semiconducting material and it can be used to build certain functional cuircuit which can be further sent for fabrication. 
![ ](Images/4.png)
<br /> There is difference between Macros and IPs. There are some files which foundry will give to us. With this, we can communicate with Foundry.
<br />•	Consider macro cells as pieces of logic blocks, mainly intellectual properties (IP), which can be used in a design without the need to (of) building them from scratch.
<br />•	Macro-cells in the integrated circuits (IC) design are large blocks which can be viewed as black-boxes. The logic and electronic behavior of these macro-cells are given but the inside structural description may or may not be known.
<br />•	Standard-cells in the circuit have the same height and need to be placed in the specified rows. Macro-cells may be located anywhere inside the layout area. No overlap is allowed between any two cells (either macro and macro, standard and standard or macro and standard).
<br />•	An Intellectual Property (IP) core in Semiconductors is a reusable unit of logic or functionality or a cell or a layout design that is normally developed with the idea of licencing to multiple vendor for using as building blocks in different chip designs


From Software Applications to Hardware:

![ ](Images/5.png)
<br /> Compiler which converts high level language to machine language, which reflects in Layout of processor to get output. The Interface required between RISC-V architecture(Specification) and Layout is HDL. RISC-V architecture(Specification) is given to HDL(RTL is to implement specification ) and generates Layout. This is RTL to GDS flow.

<br />Introduction to RISC V

<br />•	If the Hardware we have is RISC V CPU core, the Instructions will have the format of RISC V. This task is done by the compiler which converts the incoming high level program to its respective Instructions(contents of .exe file), since the Hardware is implemented using these instructions. The Assembler takes .exe file and converts to Machine language(0&1). This Binary is fed to Hardware(RISC-V Processor) to generate the Output.
<br />•	RISC V is an instruction set architecture (ISA) rooted in reduced instruction set computer (RISC) principles. RISC-V is unique, even revolutionary, because it is a common, free, open-source ISA to which software can be ported, hardware can be developed, and processors can be built to support it. As a RISC architecture, the RISC-V ISA is a load–store architecture. Its floating-point instructions use IEEE 754 floating-point. 
![ ](Images/6.png)
<br /> •	The functionality of a device written in VHDL or Verilog is RTL. (models a synchronous digital circuit interms of flow of digital signals/data between hardware registers, and the logical operations performed on those signals).
<br /> •	RTL is synthesized to convert to gate-level description called as Netlist.

<br />SoC Design 

<br />The 3 important factors in Digital ASIC Design process are,
<br />1.	RTL Designs(Register Transfer Level) {many RTL design is available in opensource like librecores.org,openecores.org }
<br />2.	EDA Tools(Electronic Design Automation) {spice simulator, sis, magic, Qflow, openROAD, openLANE etc}
<br />3.	PDKs Data(Process Design Kits) {Google - Skywater 130nm PDK }

<br />What is a PDK?
<br />PDK stands for Process Design Kit, it is provided by foundaries and it consists of library or set of building blocks which are used to build ICs. Each component in the library is seperate building bolck and ae made following certain foundary rules.
<br />PDKs acts as an inteface between the FABs and the designeers. PDKs have collection of files whcih are used to model a fabrication process for the EDA tools used to design an IC. PDK consists of tecnology node information, Process Design Rules (to verify DRC, LVC, PEX, etc), device model, I/O libraries, Standard cell libraries, macros files, lef files, etc.
<br />Google along with SKYWater made the laters PDK opensource (130 nm node). 

<br />Environment Setup
<br />The OpenLANE flow requires various open source tools as well as their supporting tools to be installed for the complete Physical design flow. Installing this tools one by one is tedious as well as one can get lost in the steps. Installation can be done easily using some set of scripts present in following repositories VSDFlow (for installing Yosys, OpenSTA, Magic, OpenTimer, netgent, etc) and OpenLANE Build Scripts.

<br />Open Source Digital ASIC Design
![ ](Images/7.png)
<br />ASIC Design Flow :RTL2GDS Flow
![ ](Images/8.png)
<br />The flow starts from the HDL code i.e.RTL model and ends with GDSII file. The major implimenation steps are:
<br />•	Synthesis - During synthesis the HDL design is translated into circuits, which are made up of components present in the standard cell library. The resultant circuit is described in HDL and its referred as gate level netlist which is functional equivalent of RTL code. 
![ ](Images/9.png)
<br />•	Floor Planning - In Floor planning the die is partitioned into different building blocks or components, also the I/O pads are distributed. 
![ ](Images/10.png)
<br />•	Power Planning - The power network is constructed typically for a chip was it has to power multiple VDD and ground pins. The power pin are connected to all component through rings and multiple horizontal and vertical strips. Such parallel structure is meant to reduce the resistance.
![ ](Images/11.png)
<br />•	Placement – Places the cells on the Floorplan rows, aligned with the sites. Placement is done in two ways Global placement and detailed placement. Global placement provide optimal result and these may or may not be legal where as the detail placement is always legal.
![ ](Images/12.png)
<br />•	Clock Tree Synthesis (CTS) - Before signal routing clock routing is done so that the clock distribution is done to every sequential block. Clock distribution network delivers the clock to each of the sequential block. It is done so that there is minimum skew and latency. It usually follows a shape i.e., H-tree, X-tree, etc.
![ ](Images/13.png)
<br />•	Routing - The signal routing is done using metal layers. It is essential to find valid pattern of horizontal and verticle wires to implement the nets that connects the cells together. Router uses the available metal layers as defined by the PDK. For each metal layer the PDK defines the thickness, width, pitch and vias. Vias are used to connect two metal wires.
![ ](Images/14.png)


<br />Getting Familiar to Opensource EDA tools
<br />EDA or Electronics Design Automation refers to the use of computer programs and software tools for designing, simulation, layout, and verification of electronic systems. These are a set of powerful tools to physically design integrated circuits. As these ASICs are composed of billions of transistors, no human being is capable of designing these circuits without the help of automated tools. These tools assist a chip designer from RTL to GDS level (which is the last step in ASIC design flow before being sent to fabrication). EDA is not only limited to software solutions; it also includes hardware and other services used in the definition, planning, designing, simulating, implementation, verification of the design, and manufacturing of the devices. As integrated circuit technology has grown, the chip design introduces new challenges to optimize in terms of efficiency, power consumption, cost-effectiveness, size, and many more. And there are several different tools available for each step.
<br /> These EDA tools are indispensable to cope with the complexity of very large-scale integrated circuits (VLSI). On the other hand, the cost of these tools is the stumbling block that a small team will never be able to design their chip. 
<br />For this reason, freeware EDA sources are getting popular among researchers and students to learn about ICs and fabricate their chips.

<br />OpenLANE Introduction
<br />OpenLANE is a completely automated RTL to GDSII flow which embeds in it different opensource tools, namely, OpenROAD, Yosys, ABC, Magic etc., apart from many custom methodology scripts for design exploration and optimization. Openlane is built around Skywater 130nm process node and is capable of performing full ASIC implementation steps from RTL all the way down to GDSII. The flow-chart below gives a better picture of openlane flow as a whole (Image Courtesy: efabless/openlane)
![ ](Images/15.png)


<br />Overview of Physical Design flow
<br />Place and Route (PnR) is the core of any ASIC implementation and Openlane flow integrates into it several key open source tools which perform each of the respective stages of PnR. Below are the stages and the respective tools (in ( )) that are called by openlane for the functionalities as described:
<br />•	Synthesis
o	Generating gate-level netlist (yosys).
o	Performing cell mapping (abc).
o	Performing pre-layout STA (OpenSTA).
<br />•	Floorplanning
o	Defining the core area for the macro as well as the cell sites and the tracks (init_fp).
o	Placing the macro input and output ports (ioplacer).
o	Generating the power distribution network (pdn).
<br />•	Placement
o	Performing global placement (RePLace).
o	Perfroming detailed placement to legalize the globally placed components (OpenDP).
•	Clock Tree Synthesis (CTS)
o	Synthesizing the clock tree (TritonCTS).
<br />•	Routing
o	Performing global routing to generate a guide file for the detailed router (FastRoute).
o	Performing detailed routing (TritonRoute)
<br />•	GDSII Generation
o	Streaming out the final GDSII layout file from the routed def (Magic).

<br />OpenLane Directory Hierarchy:
<br />LAB Day1
<br />Step1:Starting OpenLane
<br />Go to openlane working directory
<br />divyadstvm@vsd-pd-workshop-02:~/Desktop/work/tools/openlane_working_dir/openlane$ docker
<br />bash-4.2$ pwd
<br />/openLANE_flow
<br />bash-4.2$ ./flow.tcl -interactive
<br />% package require openlane 0.9

<br />•	We use docker command to open the open lane in working directory.
<br />•	Then use ./flow.tcl -interactive which identifies using the script the flow has to move and intractive means we do a step by step process.
<br />•	Then we need to import all the package that are required to run this program by package require openlane 0.9
<br />•	NOTE Command to run fully automated run ./flow.tcl -design picorv32a
<br />![ ](Images/16.png)
<br />Synthesis using OpenLANE:
<br />Step 2: Design Preperation
<br />•	Now we would be running our first step which is synthesis in openlane but before that we need to set the file system in the design setup stage which will be setting up the data for our data structure for our design.
<br />•	For this, enter below command in openlane :
<br />% prep -design picorv32a 
<br />When it is done it will show:
<br />[INFO]: Preparation complete
<br />•	So after this the runs directory has been created into picorv32a directory under which folder structures required by the openlink will be created in which all the folders will be empty except tmp

<br />Step 3: Running Synthesis
<br />For this, enter below command in openlane :
<br />% run_synthesis
<br />When it is done it will show:
<br />[INFO]: Synthesis was successful

<br />TASK 1 : Finding the d flip flop ratio of the design created
<br />Flop ratio is defined as the ratio of number of D flip flop to total number of cell.
<br />Flop Ratio of design  = No: of D-FF / Total No: of cells
<br />= sky130_fd_sc_hd_dfxt p-2 / No: of Cells  = 1613 / 14876 = 0.1084 = 10.84 %

<br />Synthesis Report showing No: of D-FF = 1613 

![ ](Images/17.png)
<br />Synthesis Report showing No: of Cells  = 14876
![ ](Images/18.png)
![ ](Images/19.png)
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br /> •	Day 2: Good Floorplan vs bad Floorplan and Introduction to Library Cells
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

<br /> •	Day 3 - Design library cell using Magic Layout and ngspice characterization
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

<br /> •	Day 4 - Pre-layout timing analysis and importance of good clock tree
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

<br /> •	Day 5 – Final Steps for RTL2GDS using TritonRoute and OpenSTA
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

