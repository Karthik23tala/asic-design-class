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

![image](https://github.com/user-attachments/assets/af89f6fb-f34b-4204-a68a-8ea0e933a3c6)

![image](https://github.com/user-attachments/assets/efcb189c-bd62-4bc9-bb00-e26ef92fb562)

![image](https://github.com/user-attachments/assets/432df406-a0f9-4982-a776-9f572f5d3eef)

Command to view the floorplanning:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

![image](https://github.com/user-attachments/assets/7563bdab-1743-45f0-8d61-5b977735e1aa)

![image](https://github.com/user-attachments/assets/48ea0abf-1ff7-4879-9192-8d3fda5388b5)

![image](https://github.com/user-attachments/assets/32da2071-9b15-47c7-931b-b95b7da4517a)

Floorplan Log File:

![image](https://github.com/user-attachments/assets/9ca3d628-641a-453c-9541-2c1333ef3d14)

Floorplan Timing Report:

![image](https://github.com/user-attachments/assets/553f4d1a-c64e-49a2-b7ef-ba5616ce7f00)

```

==========================================================================
floorplan final report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
floorplan final report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
floorplan final report_worst_slack
--------------------------------------------------------------------------
worst slack INF

==========================================================================
floorplan final report_checks -path_delay min
--------------------------------------------------------------------------
No paths found.

==========================================================================
floorplan final report_checks -path_delay max
--------------------------------------------------------------------------
No paths found.

==========================================================================
floorplan final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_reset_a3$_DFF_P_ (rising edge-triggered flip-flop)
Endpoint: core.CPU_Xreg_value_a4[10][13]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop)
Path Group: unconstrained
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.00    0.00    0.00 ^ core.CPU_reset_a3$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   591    1.46   13.42    9.67    9.67 ^ core.CPU_reset_a3$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_reset_a3 (net)
                 13.42    0.00    9.67 ^ _10124_/A (sky130_fd_sc_hd__inv_1)
   464    1.04    0.00   21.17   30.85 v _10124_/Y (sky130_fd_sc_hd__inv_1)
                                         _04513_ (net)
                  0.00    0.00   30.85 v _10227_/A (sky130_fd_sc_hd__nand3_1)
    31    0.08    0.78    0.52   31.37 ^ _10227_/Y (sky130_fd_sc_hd__nand3_1)
                                         _04613_ (net)
                  0.78    0.00   31.37 ^ _10232_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.20    0.22   31.59 v _10232_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _00548_ (net)
                  0.20    0.00   31.59 v core.CPU_Xreg_value_a4[10][13]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                 31.59   data arrival time
-----------------------------------------------------------------------------
(Path is unconstrained)



==========================================================================
floorplan final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             7.92e-12   3.67e-12   1.45e-08   1.45e-08  58.0%
Combinational          1.01e-11   1.77e-11   1.04e-08   1.05e-08  42.0%
Clock                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  1.80e-11   2.13e-11   2.49e-08   2.49e-08 100.0%
                           0.1%       0.1%      99.8%
```

Command for Placement:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

![image](https://github.com/user-attachments/assets/b9c1a6a6-54ae-4d62-b6d0-de6ee80fd1bc)

![image](https://github.com/user-attachments/assets/6c42414e-75d2-40e7-9292-ece8583812f1)

![image](https://github.com/user-attachments/assets/4a0e619c-fed5-44c8-8730-18b35d07246a)


```
make gui_place
```

![image](https://github.com/user-attachments/assets/01796517-c2da-4138-8587-a014958b2cc4)

![image](https://github.com/user-attachments/assets/3d95620b-85a0-4e71-a718-361ee94f4bd0)

Timing Report:

![image](https://github.com/user-attachments/assets/22a2ddae-7527-4449-9ff6-d5efd0d18003)

```

==========================================================================
detailed place report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
detailed place report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
detailed place report_worst_slack
--------------------------------------------------------------------------
worst slack INF

==========================================================================
detailed place report_checks -path_delay min
--------------------------------------------------------------------------
No paths found.

==========================================================================
detailed place report_checks -path_delay max
--------------------------------------------------------------------------
No paths found.

==========================================================================
detailed place report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_taken_br_a4$_DFF_P_ (rising edge-triggered flip-flop)
Endpoint: core.CPU_Xreg_value_a4[15][18]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop)
Path Group: unconstrained
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.76    0.00    0.00 ^ core.CPU_valid_taken_br_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.04    0.46    0.46 v core.CPU_valid_taken_br_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_valid_taken_br_a4 (net)
                  0.04    0.00    0.46 v _07913_/B (sky130_fd_sc_hd__or4_4)
    41    0.23    0.36    0.84    1.30 v _07913_/X (sky130_fd_sc_hd__or4_4)
                                         _02930_ (net)
                  0.36    0.02    1.31 v load_slew103/A (sky130_fd_sc_hd__buf_16)
    36    0.28    0.15    0.35    1.66 v load_slew103/X (sky130_fd_sc_hd__buf_16)
                                         net103 (net)
                  0.15    0.01    1.68 v max_cap102/A (sky130_fd_sc_hd__buf_16)
    24    0.27    0.14    0.26    1.94 v max_cap102/X (sky130_fd_sc_hd__buf_16)
                                         net102 (net)
                  0.16    0.04    1.97 v _07915_/A (sky130_fd_sc_hd__clkinv_16)
    43    0.48    0.31    0.25    2.22 ^ _07915_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _02932_ (net)
                  0.37    0.11    2.33 ^ _09981_/C (sky130_fd_sc_hd__nor3_2)
     2    0.02    0.10    0.13    2.46 v _09981_/Y (sky130_fd_sc_hd__nor3_2)
                                         _04371_ (net)
                  0.10    0.00    2.46 v _09982_/B1 (sky130_fd_sc_hd__a21oi_4)
     6    0.10    0.69    0.57    3.02 ^ _09982_/Y (sky130_fd_sc_hd__a21oi_4)
                                         _04372_ (net)
                  0.69    0.00    3.02 ^ _09988_/A2 (sky130_fd_sc_hd__o21ai_4)
    16    0.12    0.32    0.36    3.39 v _09988_/Y (sky130_fd_sc_hd__o21ai_4)
                                         _04378_ (net)
                  0.32    0.00    3.39 v _11217_/A (sky130_fd_sc_hd__nor3_4)
    14    0.11    1.07    0.96    4.35 ^ _11217_/Y (sky130_fd_sc_hd__nor3_4)
                                         _05443_ (net)
                  1.07    0.00    4.35 ^ wire20/A (sky130_fd_sc_hd__buf_8)
    10    0.11    0.19    0.30    4.65 ^ wire20/X (sky130_fd_sc_hd__buf_8)
                                         net20 (net)
                  0.20    0.01    4.66 ^ _11238_/B (sky130_fd_sc_hd__nand2_4)
     5    0.04    0.22    0.13    4.79 v _11238_/Y (sky130_fd_sc_hd__nand2_4)
                                         _05460_ (net)
                  0.22    0.00    4.79 v _11253_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.23    0.23    5.02 ^ _11253_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _00713_ (net)
                  0.23    0.00    5.02 ^ core.CPU_Xreg_value_a4[15][18]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.02   data arrival time
-----------------------------------------------------------------------------
(Path is unconstrained)



==========================================================================
detailed place report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
detailed place max_slew_check_slack
--------------------------------------------------------------------------
0.06809719651937485

==========================================================================
detailed place max_slew_check_limit
--------------------------------------------------------------------------
1.4951549768447876

==========================================================================
detailed place max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0455

==========================================================================
detailed place max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_capacitance_check_slack
--------------------------------------------------------------------------
0.007044170051813126

==========================================================================
detailed place max_capacitance_check_limit
--------------------------------------------------------------------------
0.19410200417041779

==========================================================================
detailed place max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.0363

==========================================================================
detailed place max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
detailed place max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
detailed place max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
detailed place setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
detailed place hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
detailed place report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
No paths found.

==========================================================================
detailed place report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
No paths found.

==========================================================================
detailed place critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path delay
--------------------------------------------------------------------------
-1

==========================================================================
detailed place critical path slack
--------------------------------------------------------------------------
0

==========================================================================
detailed place slack div critical path delay
--------------------------------------------------------------------------
0.000000

==========================================================================
detailed place report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             9.16e-12   1.10e-11   1.45e-08   1.46e-08  49.9%
Combinational          1.89e-11   4.94e-11   1.46e-08   1.46e-08  50.1%
Clock                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  2.80e-11   6.04e-11   2.91e-08   2.92e-08 100.0%
                           0.1%       0.2%      99.7%
```
Command for CTS:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

![image](https://github.com/user-attachments/assets/8728531e-52f8-4d7c-b5bc-6f62b85dbfba)

```
make gui_cts
```

![image](https://github.com/user-attachments/assets/7d4c2181-9c74-4278-8c7c-1a79cb50d1d4)

![image](https://github.com/user-attachments/assets/0e5231f9-54ba-47d4-b43f-0cb59e560a6b)

![image](https://github.com/user-attachments/assets/0df60fd1-343b-41be-b29d-710681af3b22)

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

Command for Route:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```

![image](https://github.com/user-attachments/assets/dc7a49cd-ea40-4a24-815f-28c05362440a)

![image](https://github.com/user-attachments/assets/090bd5cb-8e66-4b8c-8ab1-18275ab1eac2)

GUI for Route:

```
make gui_route
```

![image](https://github.com/user-attachments/assets/36757b0e-f655-4163-92ad-4701dba80725)

![image](https://github.com/user-attachments/assets/a554b4cb-4d24-4363-bcba-eec33d7c08a5)
