# VLSI-SoC-Physical-Design-Using-OpenSource-EDA-Tools

**Day1 : Inception of Open Source EDA, OpenLANE and SKY130 PDK :**

![](Blogs4p-Rep/Soc.jpg)

ASIC Design consist of 3 elements:
1. RTL IP's
2. EDA Tools
3. PDK Data 

PDK (Process Design Kit ) is interface between the FAB and the designer.
It is collection of files used to model a fabrication rocess for EDA Tools used to design an IC

Currently we will be using Google + Skywater Technology based OpenSorce PDK know as Sky130 PDK
Also we will be using OpenSource tool known as OpenLANE which is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII.

![](Blogs4p-Rep/OpenLane%20flow.jpg)

Place and Route (PnR) is the core of any ASIC implementation and Openlane flow integrates into it several key open source tools which perform each of the respective stages of PnR. Below are the stages and the respective tools (in ( )) that are called by openlane for the functionalities as described:

* Synthesis
   * Generating gate-level netlist (yosys).
   * Performing cell mapping (abc).
   * Performing pre-layout STA (OpenSTA).
* Floorplanning
   * Defining the core area for the macro as well as the cell sites and the tracks (init_fp).
   * Placing the macro input and output ports (ioplacer).
   * Generating the power distribution network (pdn).
* Placement
   * Performing global placement (RePLace).
   * Performing detailed placement to legalize the globally placed components (OpenDP).
* Clock Tree Synthesis (CTS)
   * Synthesizing the clock tree (TritonCTS).
* Routing
   * Performing global routing to generate a guide file for the detailed router (FastRoute).
   * Performing detailed routing (TritonRoute)
* GDSII Generation
   *Streaming out the final GDSII layout file from the routed def (Magic).

Below is the simplified RTL to GDSII flow 

![](Blogs4p-Rep/RTL2GDS.jpg)

* Synthesis : Converts RTL to a circuit out of components from Standard Cell library (SCL). Standard Cells have regular layout fixed height width is varaible but discrete.
* Floor and Power Planning : Objective here is to plan silicon area and create robust power distribtion network to power the circuit.
* Chip Floor Planning : Partition the chip die between different system building blocks and place the IO pads.
* Macro Floor Planning : Macro Dimension, pin location and row definition.
* Power Planning : Power network is contructed with multiple VDD and GND lines, such parallel structures are meant to reduce the resistance and hence the IR drop. Uses upper metal layer and hence less resistance.
* Placement : Place the cells on the floorlan rows align within the sites, place close to each other to reduce inter-connect delay and also to enable sucessful routing.
* Clock Tree Sysnthesis (CTS) : Create a clock distribution network to deliver the clock to all sequential elements with minimum skew.
* Routing : Implement the interconnect using available metal layer. Usually done in 2 steps Global Routing and Detailed Routing.
* Sign-off : Physical Verification is carried out sing DRC ( Deisgn Rule Check ) and LVS (Layot verus Schematic ) Check  during this timing verification STA Static Timing Analayis is also carried out.

**OPENLANE**
  * Started as Open Source automated RTL to GDSII flow for a True Open Source Tape Out Experience.
  * Main Goal is to produce clean GDSII with no hman intervention.
  * Tuned for Skywater 130nm Open PDK.
  
**Main Objective : The goal of the project is to design a single height standard cell and plug this custom cell into a more complex design and perform it's PnR in the openlane flow. The standard cell chosen is a basic CMOS inverter and the design into which it's plugged into is a pre-built picorv32a core.**
  
PicoRV32 is a CPU core that implements the RISC-V RV32IMC Instruction Set. It can be configured as RV32I, RV32IC, RV32IM, or RV32IMC core; where the suffixes stand for:

M - Multiply extension
I - Base Integer Instructions
C - Compressed Instructions
PicoRV32 is free and open hardware licensed under the ISC license.


  **DAY1_LAB** 
  
  * OpenLANE directory structure in detail.
  * Design preparation step using following commands</br>
    ./flow.tcl -interactive : which opens OPENLANE</br>
    package require openlane 0.9</br>
    prep -design picorv32a</br>
    run_sythesis
    
    Design was selected and Synthesis was carried out.
  
  ![](Blogs4p-Rep/D11.jpg)
  ![](Blogs4p-Rep/D12.jpg)
  ![](Blogs4p-Rep/D13.jpg)
  ![](Blogs4p-Rep/D14.jpg)

**Day2 : Floorplan and Library Cells**

**DAY2_LAB**

In this lab follwoing were the stage :
* Stage 1 : 
    * Data preparation stage :</br>
       Open Openlane </br>
       How to overwrite the file </br>
       How to check and set the parameters such as CLOCK PERIOD on fl</br>
       To run_synthesis</br>
       
* Stage 2 : Floorlan </br>
     * To read switches varaibles in floorlan.tcl file </br>
       To  set the IO mode </br>
       To knowwhich file is give the priority in terms to set the varaibles (Highest Priority to SKy130A file second Priority to Config.tcl file and lowest priority to floorplan.tcl file located in Openlan_flow/configuration </br>
       To run_floorplan </br>
       To open Magic tool to observe the floorplan </br>
       
Observation in Floorplan : </br>
1. Inuts and Outputs ins are equidistant </br>
2. By selecting the Horizontal and vertical pin : we can come to know which metal layer it has</br>
3. We can observe decap cells at the peripheral of the IO pins
4. We can also observe tap cells : used to avoid latch upconditions in CMOS devices.
5. Standard Cells are located at the lower left corner

* Stage 3 : Placement </br>
    * run_placement

![](Blogs4p-Rep/D21.jpg)
![](Blogs4p-Rep/D22.jpg)
![](Blogs4p-Rep/D23.jpg)
![](Blogs4p-Rep/D24.jpg)
![](Blogs4p-Rep/D25.jpg)
![](Blogs4p-Rep/D26.jpg)
![](Blogs4p-Rep/D27.jpg)
![](Blogs4p-Rep/D28.jpg)
![](Blogs4p-Rep/D29.jpg)
![](Blogs4p-Rep/D211.jpg)
![](Blogs4p-Rep/D212.jpg)
![](Blogs4p-Rep/D213.jpg)
       

**Day3 : Design Library Cell using Magic Layout and ngSpice Characterization:**

**DAY3_LAB**

In this lab we clone the CMOS Inverter from vsdstdcelldesign</br>
We check the layot of the design with resect to NMOS MOS and its connections with the metal layers.</br>
Then we extract the ngsice netlist</br>
We also extract all the parasitic capacitors</br>
We also need to modify the spice deck file according to the gird value NMOS and PMOS definition in the vsdstdcelldesign/libs</br>
Then to simulate the ngSPICE netlist</br></br>
Characterized the cell using 4 parameters </br>
1. Rise time delay: Rise time of the Outut waveform from 20% to 80%
2. Fall time delay: Fall time of the Outut Waveform from 80% to 20%
3. Fall Cell delay : It is difference between the time period the output falls to 50% and when input rise to 50%
5. Rise Cell delay : It is difference between the time period the outut rise to 50% and when input falls to 50%

Rise time delay | Fall time delay | Fall Cell delay | Rise Cell delay |
|:---:|:---:|:---:|:---:|
|0.06368nsec|0.042nsec|0.028nsec|0.06nsec|

![](Blogs4p-Rep/D31.jpg)
![](Blogs4p-Rep/D32.jpg)
![](Blogs4p-Rep/D34.jpg)

**Day4 : PreLayout Timing Analysis and Clock Tree Synthesis:**

1. We convert grid info into the track info
2. To include the standard cell into the design we dont require PWR, GND, Port information, Logic part.
3. The only information we require is inner boundary of the cell, PWR  GND and Inut and Output Port.
4. LEF (Library Echange format) comes into picture  LEF file has these information.
5. LEF file protect the IP's logic.
6. For PnR we need to make follwoing guidelines to make the Standard cell set
  * Input and Output ports must lie on the intersection of vertical and horizontal tracks.
  * The width of the standard cell shold be odd multiple of track horizontal pitch and height should be odd multiple of track vertical pitch.
7.Converge grid definition to track definition.
8.Define the Ports  as pins of the macro.
9. Next objective will be what is the purpose of the port means how doe the tool know that A is input port and Y is output port, VPWR is power and VGND is ground these definition has to be done using **port class and ort use**.
10.Now extract the LEF file and plug this file the the design file.
11. In the Openlane Flow we need to include our custom cell and 1st stage in OpenLane is snythesis the ABC flo of the synthesis mst ma the netlist to the cells int he library so we need to have the libray which our cell definition for synthesis.
12. Before synthesis add and merge the LEF files</br>
./flow.tcl -interactive</br>
package require openlane 0.9</br>
prep -design picorv32a  -tay Day2 -overwrite</br>
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]</br>
add_lefs -src $lefs</br>
run_sythesis</br>
Once we run the synthesis we can observe the huge slack</br>

Slack | tns (total negative slack) | wns (worst negative slack)|
|:---:|:---:|:---:|
|-17.96|-2593.43|-17.96|

12. Configure the Synthesis setting to fix the slack</br>
So after synthesis set follwoing parameters</br>
SYNTH_STRATEGY = 1</br>
SYNTH_BUFFERING = 1</br>
SYNTH_SIZING = 1</br>
SYNTH-DRIVING_CELL = sky130_fd_sc_hd_inv8</br>
rn_synthesis again and observe the slack as below ( has a hude improvement )</br>

Slack | tns (total negative slack) | wns (worst negative slack)|
|:---:|:---:|:---:|
|-3.17|-36.81|-3.17|

Day5 : RTL2GDS using TritonRoute and OpenSTA




