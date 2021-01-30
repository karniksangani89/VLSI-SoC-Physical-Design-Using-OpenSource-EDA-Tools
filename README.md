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
   * Perfroming detailed placement to legalize the globally placed components (OpenDP).
* Clock Tree Synthesis (CTS)
   * Synthesizing the clock tree (TritonCTS).
* Routing
   * Performing global routing to generate a guide file for the detailed router (FastRoute).
   * Performing detailed routing (TritonRoute)
* GDSII Generation
   *Streaming out the final GDSII layout file from the routed def (Magic).

Below is the simplified RTL to GDSII flow 

![](Blogs4p-Rep/RTL2GDS.jpg)







Day2 : Floorplan and Library Cells

Day3 : Design Library Cell using Magic Layout and ngSpice Characterization.

Day4 : PreLayout Timing Analysis and Clock Tree Synthesis.

Day5 : RTL2GDS using TritonRoute and OpenSTA




