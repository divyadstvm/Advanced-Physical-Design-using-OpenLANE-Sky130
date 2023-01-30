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

<br /> •	Day 2: Good Floorplan vs bad Floorplan and Introduction to Library Cells
<br />The placement of logical blocks, library cells, and pins on a silicon chip is known as chip floorplanning. It ensures that every module has been given the proper area and aspect ratio, that every pin of the module is connected to another module or the chip's edge, and that modules are placed so that they take up the least amount of space on a chip.
<br />1.	The height and width of core and die
<br />•	The core, which is located in the middle of the die, is where the logic blocks are placed. The dimensions of each standard cell on the netlist determine the width and height of Core.
![ ](Images/20.png)
<br />•	Utilization Factor is defined as the ratio of area of occupancy by the netlist to total area of the core. Utilization factor in a realistic situation is between 0.5 and 0.6. Only this space is used for the netlist; the rest space is used for routing and more extra cells.
<br />•	Aspect Ratio is defined as the ratio between height and the width of core.
<br />2.	The location of Preplaced Cell
<br />•	These are complex logic blocks that are previously implemented but can be reused, such as memory, clock-gating cells, muxes, comparator, etc. Prior to placement and routing, the user-defined placement on the core must be completed (thus preplaced cells).
<br />•	This needs to be very well described because the automated place and route tools won't be able to touch or move these preplaced cells.
<br />3.	Surround preplaced cells with decoupling capacitors
<br />•	The complex preplaced logic block needs a lot of current from the power supply to switch the current. However, due to the resistance and inductance of the wire, there will be a voltage drop because of the distance between the main power supply and the logic block. As a result, the voltage at the logic block might no longer fall within the noise margin range (logic is unstable).
<br />•	Utilizing decoupling capacitors which are hudge bunch of capacitor completely filled with charge, close to the logic block will provide the necessary current for the logic block to switch inside the desired noise margin range.
![ ](Images/21.png)
<br />4.	Power Planning
<br />•	It is not possible to apply a decoupling capacitor for sourcing logic blocks with enough current throughout the entire chip, only on the important components (preplaced complex logicblocks).
<br />•	Due to the large amount of current that must be sinked simultaneously when a large number of elements switch from logic 1 to logic 0, this could result in ground bounce, and switching from logic 0 to logic 1 could result in voltage droop because there is not enough current from the power source to source the needed current for all elements. The increase or decrease in voltage may not be within the noise margin range due to voltage droop and ground bounce.
<br />•	The reason for problem of voltage droop and ground bounce is because the supply has been provided only from one point so we use multiple power source taps (power mesh) are the solution, allowing components to source current from the closest VDD tap and sink current to the closest VSS tap. The majority of processors include several powersource pins because of this.
![ ](Images/22.png)
<br />5.	Pin Placement
<br />•	The area between the core and the die is where the input and output ports are located.
<br />•	The positions of the ports depend on the placements of the cells that are connected with them on the core.
<br />•	Since this clock must be able to drive the entire chip so the clock pin is thicker (lowest resistance route) than data ports.
![ ](Images/23.png)
<br />6.	Logical Cell Placement Blockage
<br />This ensures that no cells are placed by the automated placement and routing tool on the die's pin locations.
![ ](Images/24.png)
<br />Floorplanning using OpenLANE
<br />Steps to run and view Floorplan using OpenLANE
<br />•	The configuration variables or switches must be set up before to starting the floorplan stage..
<br />•	The configuration variables location is
<br />Step 1: Running floorplan
<br />•	We have lot of switches with which we adjust the flow directory. These switches are used to set certain parameter in each stage of the flow. For eg: In the Floorplanning stage we have FP_CORE_UTIL {for utilization percentage}, FP_ASPECT_RATIO {sets the aspect ratio}, FP_CORE_MARGINS {offset b/w die boundary and core boundary}, etc. We have certain .tcl file in OpenLane which has these switchs that sets these specifications.
<br />•	The command to run floorplan is:
<br />% run_floorplan
<br />Step 2: Review floorplan files and steps to view floorplan
<br />•	Reviewing files

<br />TASK 2: Calculating the die area

<br />Solution of Task:
<br />DIE AREA(00)(660685  671405)
<br />Area of Chip = 660.685 / 671.405 = 0.984 um2
<br />The report showing die area is shown below:
![ ](Images/25.png)
<br />ioPlacer.log defines the metal layers for the I/Os.
<br />Step 3: Review floorplan layout in Magic
<br />•	Using Magic tool to view the def file
<br />The following command can be used to invoke magic tool as well as to open the def file:
<br />divyadstvm@vsd-pd-workshop-02:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-01_17-19/results/floorplan$magic -T /home/divyadstvm/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
<br />The generated floorplan layout is shown below:
![ ](Images/26.png)
<br />To center the view, press "s" to select whole die then press "v" to center the view. Point the cursor to a cell then press "s" to select it, zoom into it by pressing 'z". 
<br />Zoom in view of floorplan :
![ ](Images/27.png)
<br />Zoom in view of Standard CellArea:
![ ](Images/28.png)
<br />tkcon console :
<br />Type "what" in tkcon to display information of selected object. These objects might be IO pin, decap cell, or well taps as shown below.
![ ](Images/29.png)
<br />Placement using OpenLANE
<br />Steps to run and view Placement using OpenLANE
<br />After floorplanning, next comes placement, it determines location of each of the components on the die. 
<br />Step 1: Running Placement
<br />•	The command to run Placement is:
<br />•	%run_placement

<br />View the placement in magic
<br />For visualising the layout following a placement, use the Magic Layout Tool. The following three files are necessary in order to examine a floor layout in Magic
<br />•	Technology File sky130A.tech
<br />•	Merged LEF file merged.lef
<br />•	vim picorv32a.floorplan.def files

<br />To open layout in magic use this command in this location:
<br />divyadstvm@vsd-pd-workshop-02:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-01_19-15/results/placement$ magic -T /home/divyadstvm/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
<br />-T <address_of_sky130A.tech_file> where T is for technology file lef read <address_of_merged.lef_file> to read the lef files (we use lef read because its a standard industry file) def read <address_of_picorv32a.placement.def_file> to read the def files (we use def read because its a standard industry file)

<br />NOTE
<br />•	 If address of the required file is at the same working location then we just need to provide the required file name. 
<br />•	This sky130A.tech(technology), merged.lef(layout exchange format) and picorv32a.placement.def(design exchange format) files comes along with the pdk of sky130.

<br />The generated floorplan layout is shown below:
![ ](Images/30.png)
<br />Zoom in view of Placement :
![ ](Images/31.png)
<br />Zoom in view of Placement :
![ ](Images/32.png)

<br />Cell Design Flow
<br />•	In Cell design we will look at how a standard cell is designed in the library
<br />![ ](Images/33.png)
<br />•	Cell height has been defined as the seperation between power and the ground rail and it is the responsibilty of the cell develepor that cell height is mantained. Cell height depends on the timing information(if the cell height is high then it would be able to drive more longer wire, that is called higher drive strength cells)
<br />•	The standard cells has to operate at a certain Supply Voltage which is being provided by the top level designer and accordingly the library developer has to take that supply voltage and design the library cell such that it specifies supply voltage.
<br />•	Metal Layer, Pin Locations, Drawn Gate Length requirments has to be decided by the library developer.
Characterization
<br />•	Next step after we get the extraced step netlist and layout is characterization.
<br />•	Characterization helps us to get timing, noise and power information.
<br />•	The output of characterization is timing, noise, power.lib files and the functionality of this circuit.

<br />Characterization Flow:
<br />•	Read in the models files
<br />•	Read the extraced spice netlist
<br />•	Recognize the behaviour of the buffer
<br />•	Read the sub circuits of the inverter
<br />•	Attach the necessary power sources
<br />•	Apply the stimulus
<br />•	Provide the necessary output capacitances
<br />•	Prove the necessary simulation commands
<br />![ ](Images/34.png)

<br /> •	Day 3 - Design library cell using Magic Layout and ngspice characterization
<br />To go in depth of one of the cells(inverter cell), we won't build it from scratch rather we would use the github to get the .mag(magic) files and from there we will be doing Post Layout simulation in ngspice and post characterizing our sample cell, we would be plugging this cell into a OpenLANE flow, into picorv32a core.
<br />NOTE - In IO placement in floorplan
<br />On OpenLANE, configurations can be modified while in flight. On OpenLANE, for instance, use % set ::env(FP_IO_MODE) 2 to make IO mode not equidistant. On mode 2, the IO pins won't be evenly spaced out (default of 1). View the def layout for magic by launching floorplan once more with % run floorplan. The configuration will only be available for the current session if it is changed on the fly; it will not be changed in runs/config.tcl, echo $::env(FP_IO_MODE) to output the variable's most recent value.
<br />SPICE deck creation for CMOS inverter
<br />First we need to design the library cells:
<br />•	The CMOS inverter's SPICE deck represents component connections (essentially a netlist).
<br />•	SPICE deck values equal the W/L value (0.375u/0.25u), which denotes a width of 375nm and a length of 250nm. PMOS should to be 2 or 3 times broader in width than NMOS. Typically, the gate and supply voltages are multiples of length (in the example, gate voltage can be 2.5V)
<br />•	Place nodes around each component and give it a name. In SPICE, a component can be identified using this.
<br />SPICE Deck Netlist Description
<br />![ ](Images/35.png)
<br />•	PMOS and NMOS descriptor syntax
<br />o	[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]
<br />•	Based on nodes and their values, all components are described.
<br />SIMULATION COMMANDS
<br />•	.op .dc Vin 0 2.5 0.05 is the start of SPICE simulation operation where Vin will be sweep from 0 to 2.5 with 0.05 steps
<br />•	tsmc_025um_model.mod is the model file which contain the technological parameters of the 0.25um NMOS and PMOS Devices.
<br />Labs for CMOS inverter ngspice simulations
<br />Steps to simulate in SPICE:
<br />source [filename].cir
<br />run
<br />setplot 
<br />dc1 
<br />plot out vs in

<br />Standard cell design and characterization using openlane flow

<br />CMOS Inverter Design using Magic

<br />Magic Tool offers a very user-friendly interface for designing the different layers of the layout. Additionally, it features a built-in DRC check feature.
To Clone vsdstdcelldesign Folder from Github:
<br />•	This will create "vsdstdcelldesign" directory which contain the sky130_inv.mag and from there we will do post layout simulation.
<br />•	The technology file which is used as an input is sky130A.tech.
<br />To Copy sky130A.tech file  to folder vsdstdcelldesign:
<br />divyadstvm@vsd-pd-workshop-02:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
<br />To invoke .tech and .mag files to open magic:
<br />divyadstvm@vsd-pd-workshop-02:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign$ magic -T sky130A.tech sky130_inv.mag &
<br />-T <address_of_sky130A.tech_file> where T is for technology file. Here we use not lef read or def read as in for .def or .lef file because .mag is not a standard industry file). & is use to free the next command prompt.
<br />•	NOTE If address of the required file is at the same working loaction then we just need to provide the required file name.

<br />Layout of the CMOS Inveter in magic
![ ](Images/36.png)
<br />Highlighted Section of CMOS inverter in magic shows as nmos in tkcon:
![ ](Images/37.png)
<br />To see the connection between 2 parts of a layout, click S thrice. Fig shows o/p,Y connected to Drain of both pmos & nmos.
![ ](Images/38.png)
<br />Characterizing the cell's(CMOS Inverter) slew rate and propagation delay
<br />Now to know the logical functioning of the inverte we extrace the spice and do simulation in ngspice open source tool.
<br />vsdstdcelldesign folder files before extracting ngspice:
![ ](Images/39.png)

<br />Extracting ngspice :
<br />Steps to extrace the spice file from magic
<br />•	Use thie command extract all to create an .ext(extraction) file.
<br />•	Use this command ext2spice cthresh 0 rthresh 0 then ext2spice to create the .spice file from .ext file, to be used with our ngspice tool and also extrace all the paracitic capacitances.
![ ](Images/40.png)
![ ](Images/41.png)

<br />SPICE File sky130_inv.spice is created inside Folder vsdstdcelldesign as below:
![ ](Images/42.png)
<br />sky130_inv.spice file extraced from magic
![ ](Images/43.png)
<br />We then modify the sky130_inv.spice file as shown below to be able to plot a transient response:
<br />NOTE To edit vim file in linux press i and make the changes then press Esc and the :wq! to save the changes
<br />SPICE SIMULATION OF CMOS
![ ](Images/44.png)
![ ](Images/45.png)
<br />Zoom in view of Plots got after ngspice simulation
![ ](Images/46.png)

<br />Task3:
<br />1)	Find CELL FALL TRANSITION
<br />80% of 3.3v = 2.64v  : x0 = 1.00e-9, y0 =2.65
<br />20% of 3.3v = 0.66v: x0 = 1.01e-9, y0=0.66
<br />Cell fall transition = 1.01-1.00=0.01

<br />2)FIND CELL FALL DELAY
<br />x0 = 4.07685e-09, y0 =1.65242
<br />x0 = 4.04954e-09, y0=1.65161
<br />Cell Fall Delay=4.076 – 4.049 = 0.027

<br />So now we have characterized our inverter. The next goal is to make a .lef file using this inverter architecture. We will create a unique cell by plugging this .lef into openlane and plugin in this cell into our picorv32a core.

<br /> •	Day 4 - Pre-layout timing analysis and importance of good clock tree
<br />Delay
<br />Problem:
<br />•	The capacitance or the load at the output node of each and every buffer in the complete clock tree is varying.
<br />•	Also if the load is varying the input transition is varying.
![ ](Images/47.png)

<br />Timing modelling using delay tables

<br />Timing Analysis
<br />First we take the ideal clock (clock tree is not yet build) and do the timing analysis with it. After that we will do with real clock.
<br />Pre-layout timing analysis (using ideal clock)
<br />•	SETUP TIMING ANALYSIS. Specifications Clock frequency = 1GHz and period of 1ns.
<br />We have a launch flop and capture flop and in between we the the combinational logic. We have ideal clock network i.e., clock tree is not yet built. Hence we do not have any buffer in the clock path. This is a typical scenario for hold time and setup time calculation. We send the 1st riseing clock to the launch flop (t=0ns) and the 2nd rising to the capture flop (t=1ns).
![ ](Images/48.png)
<br />The equation for setup time is:
<br />Θ < T - S – SU First basic insight is the setup delay should be less than the combinational delay. Then analysing the capture flop we see some delay due to the mux. due to jitter there is delay in the exact point of clock arrival and this variation is due to internal clock circuitary (PLL).
<br />where.
<br />•	Θ = Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop
<br />•	T = Time period, also called the required time
<br />•	S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock tansists to 1 so the delay due to Mux 1 must be considered, this delay is the setup time.
<br />•	SU = Setup uncertainty due to jitter which is temporary variation of clock period. This is due to non-idealities of PLL/clock source.
<br />NOTE: Things are different for hold time.
<br />We have, T = 1000 ps, S = 10 ps, U = 90 ps Hence we arrive at Θ < 0.9 ns (for our case)

