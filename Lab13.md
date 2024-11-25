## Lab13: OpenRoad Physical Design
<br>

## **OpenROAD: Physical Design Tool for Integrated Circuits**

OpenROAD is an advanced tool for the physical design of integrated circuits, enabling a streamlined process from RTL to GDSII. It supports critical stages of chip design, such as synthesis, floorplanning, placement, routing, parasitic extraction, and timing analysis.

With a focus on minimizing wire length, OpenROAD employs hierarchical placement techniques. It includes optimization capabilities for power and timing, ensuring efficient designs. Its modular design architecture allows users to extend its functionality by incorporating custom algorithms and features.

### OpenROAD Flow Controllers

The OpenROAD project includes two major flow controllers:

#### 1. OpenROAD-flow-scripts (ORFS)

ORFS provides a comprehensive set of open-source tools for automating digital ASIC design. It delivers an end-to-end flow from RTL to GDSII, featuring:

- **Design Stages**: Synthesis, placement and routing (PnR), static timing analysis (STA), design rule checking (DRC), and layout versus schematic (LVS).
- **Customizability**: Users can adapt the framework to fit specific project requirements by configuring tools and stages.
- **Physical Design Integration**: Integrates OpenROAD for advanced physical design processes, such as hierarchical placement and optimized routing.
- **Process Design Kit (PDK) Support**: Compatible with public PDKs like GF180, Skywater130, and ASAP7, along with several private PDKs under NDA.

#### 2. OpenLane

Developed by Efabless, OpenLane is another automated RTL-to-GDSII flow. It is particularly tailored for the Skywater130 MPW Program.

### Overview of the ORFS Process (RTL to GDSII)

The ORFS flow involves the following stages:

1. **Configuration**  
   Define project-specific parameters, including the target technology node, constraints, and tool configurations.

2. **Design Entry**  
   Provide input design files, typically in Verilog format, and prepare them for subsequent stages.

3. **Synthesis**  
   Convert RTL descriptions into a gate-level netlist using tools such as Yosys and ABC.

4. **Floorplanning**  
   Organize design modules within the chip area using tools like RePlAce and Capo.

5. **Placement**  
   Precisely arrange gates or cells within the floorplan using OpenROAD's placement capabilities.

6. **Routing**  
   Establish electrical connections between cells using metal wires. Tools include FastRoute and TritonRoute.

7. **Layout Verification**  
   Ensure the layout's correctness and adherence to design rules using verification tools such as Magic.

8. **GDSII Generation**  
   Generate the final chip layout in GDSII format with tools like Magic and KLayout.

[OpenROAD Official Documentation](https://theopenroadproject.org/)

Cloning the  dependacy Github repo:

```
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh
```

Building:

```
./build_openroad.sh --local
```

Verifying the installation:

```
cd flow
make
make gui_final
```

Terminal Screenshots:

![image](https://github.com/user-attachments/assets/ba58037d-6148-49f4-bcd4-9bf6a22200ab)

![image](https://github.com/user-attachments/assets/7ddf822a-2ed4-4da4-9a96-631a35c84320)

Makerfile Screenshots:

![image](https://github.com/user-attachments/assets/2f05afd2-1e52-456d-8012-68af2a91ee4a)

![image](https://github.com/user-attachments/assets/eb6df4ad-5726-4f65-a2de-10f81308a34a)

![image](https://github.com/user-attachments/assets/e4a11418-43a2-481b-94ab-7d2047ca256a)

![image](https://github.com/user-attachments/assets/8cf3486a-a73b-4ce2-87ba-a1028d150838)

![image](https://github.com/user-attachments/assets/84469488-e4e5-4856-ab0a-52d7a157b7aa)

![image](https://github.com/user-attachments/assets/fb2104b9-6f69-4906-8acb-297e060f226a)

![image](https://github.com/user-attachments/assets/e16635c0-56ff-4656-baa0-b513d7f54b33)

![image](https://github.com/user-attachments/assets/daaa0619-485f-45aa-b795-ea9a7a4d01fd)

**Automated RTL-to-GDS Flow for VSDBabySoC**

Initial Setup:
Create Required Directories

Inside OpenROAD-flow-scripts/flow/designs/sky130hd, create a new directory named vsdbabysoc.
Similarly, create another directory named vsdbabysoc within OpenROAD-flow-scripts/flow/designs/src. Place all Verilog files for VSDBabySoC in this directory.
Copy Supporting Files

Copy the folders gds, include, lef, and lib from the VSDBabySoC directory on your system into the newly created vsdbabysoc directory.
The contents of these folders should include the following:
gds Folder: Contains avsddac.gds and avsdpll.gds.
include Folder: Contains the files sandpiper.vh, sandpiper_gen.vh, sp_default.vh, and sp_verilog.vh.
lef Folder: Contains avsddac.lef and avsdpll.lef.
lib Folder: Contains avsddac.lib and avsdpll.lib.
Add Constraints File

Copy the constraints file (vsdbabysoc_synthesis.sdc) from the VSDBabySoC directory on your system into the vsdbabysoc directory.
Include Configuration Files

Copy the configuration files macro.cfg and pin_order.cfg from the VSDBabySoC directory on your system into the same directory.

```
export DESIGN_NICKNAME = vsdbabysoc
export DESIGN_NAME = vsdbabysoc
export PLATFORM    = sky130hd

# export VERILOG_FILES_BLACKBOX = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/IPs/*.v
# export VERILOG_FILES = $(sort $(wildcard $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/*.v))
# Explicitly list the Verilog files for synthesis
export VERILOG_FILES = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/vsdbabysoc.v \
                       /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/module/rvmyth.v \
                       /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/clk_gate.v

export SDC_FILE      = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/sdc/vsdbabysoc_synthesis.sdc

#export DIE_AREA   = 0 0 1500 1500
# export CORE_AREA  = 10 10 2910 3510

# export PLACE_DENSITY ?= 0.23

export vsdbabysoc_DIR = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc

export VERILOG_INCLUDE_DIRS = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/include

# export SDC_FILE      = $(wildcard $(vsdbabysoc_DIR)/sdc/*.sdc)
export ADDITIONAL_GDS  =/home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/gds
export ADDITIONAL_LEFS  = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/lef

export ADDITIONAL_LIBS = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/lib/avsddac.lib \
				         /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/lib/avsdpll.lib
# Clock Configuration (vsdbabysoc specific)
# export CLOCK_PERIOD = 20.0
export CLOCK_PORT = CLK
export CLOCK_NET = $(CLOCK_PORT)

# Floorplanning Configuration (vsdbabysoc specific)
export FP_PIN_ORDER_CFG = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/pin_order.cfg
export FP_SIZING = absolute
export DIE_AREA = 0 0 2000 2000
export CORE_AREA = 10 10 1800 1800


# export PL_RESIZER_HOLD_MAX_BUFFER_PERCENT = 80
# export PL_RESIZER_HOLD_MAX_BUFFER_COUNT = 5000  # Set the buffer limit to a higher value if needed
# export GLB_RESIZER_HOLD_MAX_BUFFER_PERCENT = 80

# Hold Slack Margin Configuration
# export PL_RESIZER_HOLD_SLACK_MARGIN = 0.01
# export GLB_RESIZER_HOLD_SLACK_MARGIN = 0.01


export BOTTOM_MARGIN_MULT = 50
export TOP_MARGIN_MULT = 50
export LEFT_MARGIN_MULT = 200
export RIGHT_MARGIN_MULT = 200

# Placement Configuration (vsdbabysoc specific)
export MACRO_PLACEMENT_CFG = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/VSDBabySoC/macro.cfg

# Magic Tool Configuration
export MAGIC_ZEROIZE_ORIGIN = 0
export MAGIC_EXT_USE_GDS = 1

export SYNTH_HIERARCHICAL = 1

# export RTLMP_BOUNDARY_WT = 0
#  MACRO_PLACE_HALO = 100 100
# export MACRO_PLACE_CHANNEL = 200 200

# CTS tuning
# export CTS_BUF_DISTANCE = 600
# export SKIP_GATE_CLONING = 1

# export SETUP_SLACK_MARGIN = 0.2

# This is high, some SRAMs should probably be converted
# to real SRAMs and not instantiated as flops
# export SYNTH_MEMORY_MAX_BITS ?= 42000

```

![image](https://github.com/user-attachments/assets/3850fbc8-a1d9-41e5-8eef-a9c63890dbc0)

![image](https://github.com/user-attachments/assets/6148f560-c331-429a-a098-f81e7bbe16a8)

Terminal Command and Screenshots for Synthesis:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

![image](https://github.com/user-attachments/assets/3009fc6a-2d35-44eb-bcf7-a6763c1f2bab)

Synthesis Netlist:

![image](https://github.com/user-attachments/assets/e0e2fd1f-7788-4bc0-8f17-6ff4ab23b357)

Synthesis Log:

![image](https://github.com/user-attachments/assets/7f6f1da3-96d3-4a8a-a3e2-1110f891addd)

Synthesis Check:

![image](https://github.com/user-attachments/assets/674065b7-bd3b-46c3-b1d8-07e77700d1ad)

Synthesis Stats:

![image](https://github.com/user-attachments/assets/6ec050f7-47d0-44d0-ae44-4fc90b5b0dfb)

![image](https://github.com/user-attachments/assets/0430ee6d-d708-435c-9af0-792866cef180)

![image](https://github.com/user-attachments/assets/0188ea08-7d15-4a2d-9309-3ede964df067)

![image](https://github.com/user-attachments/assets/894774b7-0055-4922-b37d-06afe993f3be)

Terminal Command and Screenshots for Floorplan:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

![image](https://github.com/user-attachments/assets/8d604cbc-aee4-41c0-b293-34ef6246c327)

![image](https://github.com/user-attachments/assets/7e9e16f4-2570-4f40-9514-3f6d48d48899)

OpenROAD GUI And Review Logs For Synthesis, Floorplan and Initiating STA

Terminal Commands and Screenshots:

```
make gui_floorplan
```

![image](https://github.com/user-attachments/assets/88a4f464-3526-4287-ad54-6ccc28e2ed38)

![image](https://github.com/user-attachments/assets/1cd089aa-86ae-41da-bf0a-15e1fdbf18b5)

Command to view the floorplanning:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

![image](https://github.com/user-attachments/assets/ffff701e-bf1f-4fde-9e1a-fd18fe0b4697)

![image](https://github.com/user-attachments/assets/567cd124-aab7-4e61-98b7-3c96423b8898)

![image](https://github.com/user-attachments/assets/fe4a347f-6e50-4693-bfc7-b5974147d842)

Command for Placement:

```
sudo make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

![image](https://github.com/user-attachments/assets/0fe0adda-1127-4e09-b64d-0ab012683336)

![image](https://github.com/user-attachments/assets/de1a7937-305a-4432-ba1f-0800535d250a)

![image](https://github.com/user-attachments/assets/41b825e9-5786-4080-ae44-c2ef521029a7)

![image](https://github.com/user-attachments/assets/4f731423-8892-4dc3-aa43-25e43ede52af)


```
make gui_place
```

![image](https://github.com/user-attachments/assets/8b121897-0218-4825-b02b-7ea1fed300bf)

![image](https://github.com/user-attachments/assets/076a1ef0-7786-481d-8842-618f0e0b5d64)

Command for CTS:

```
sudo make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

![image](https://github.com/user-attachments/assets/953a767d-e5fd-46ae-8ecd-c4f808dd98dc)

```
make gui_cts
```

![image](https://github.com/user-attachments/assets/cc413d2a-eb8e-454a-9e56-ed13e3b9b2ed)

![image](https://github.com/user-attachments/assets/83d31fe4-5f9d-4954-b9e6-a1a335074913)

Clock Tree Synthesis Screenshots:

![image](https://github.com/user-attachments/assets/8350e52b-d971-4691-af55-b70207f42172)

![image](https://github.com/user-attachments/assets/f4de6fca-7d29-4ac8-8148-3a6d80a3c563)

![image](https://github.com/user-attachments/assets/ccdc7a5c-33a2-49b1-a8fa-de67c3fbdbb5)

Final Layout Commands:

```
make gui_final
```

![image](https://github.com/user-attachments/assets/17601d82-6a8f-4219-9643-a4304930a00c)

![image](https://github.com/user-attachments/assets/bd03ec92-a5a7-4ce2-b6fe-74e8e1bc00e4)

![image](https://github.com/user-attachments/assets/e0f1cad9-0cd9-4281-b995-a02c26677529)

![image](https://github.com/user-attachments/assets/bac41511-624d-42d4-92ae-1c1c920fe061)

![image](https://github.com/user-attachments/assets/0c71875b-98c3-4d07-a754-6909defa67c7)

![image](https://github.com/user-attachments/assets/056ebef4-939b-4010-8307-1f4e4de89531)

Commands to get the GTS file in the KLayout 

```
klayout -e -nn ./platforms/nangate45/FreePDK45.lyt -l ./platforms/nangate45/FreePDK45.lyp ./results/nangate45/gcd/base/6_final.gds
```

![image](https://github.com/user-attachments/assets/fc9288d7-0d7f-4d6f-8f3c-d0ba03da0b60)

![image](https://github.com/user-attachments/assets/80aff3a9-5224-4196-9d0d-a1b104a8ebbd)

Command for Route:

```
sudo make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```

![image](https://github.com/user-attachments/assets/9bb8afc3-ee13-47bf-a704-277d11933a1c)

![image](https://github.com/user-attachments/assets/0c1f6536-f0e9-4d88-8efe-ec4e5a53821a)

GUI for Route:

```
make gui_route
```

![image](https://github.com/user-attachments/assets/36757b0e-f655-4163-92ad-4701dba80725)

![image](https://github.com/user-attachments/assets/a554b4cb-4d24-4363-bcba-eec33d7c08a5)
