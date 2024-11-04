# Lab 11 Report (29/10/2024) - PVT Corner Analysis for Synthesized VSDBabySoC using OpenSTA
<br>

The PVT corner refers to the combination of Process, Voltage, and Temperature variations that a semiconductor chip might encounter during its operation. These variations can affect the performance, power consumption, and reliability of the chip, so they are simulated to ensure the chip functions correctly under different conditions

## TCL script file ```sta_pvt.tcl``` :

```
set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__tt_100C_1v80.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(9) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty /mnt/$list_of_lib_files($i)
read_liberty -min /mnt/avsdpll.lib
read_liberty -max /mnt/avsdpll.lib
read_liberty -min /mnt/avsddac.lib
read_liberty -max /mnt/avsddac.lib
read_verilog /mnt/vsdbabysoc.synth.v
link_design rvmyth
read_sdc /mnt/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /mnt/sta_output/min_max_$list_of_lib_files($i).txt

exec echo "$list_of_lib_files($i)" >> /mnt/sta_output/sta_worst_max_slack.txt
report_worst_slack -max -digits {4} >> /mnt/sta_output/sta_worst_max_slack.txt

exec echo "$list_of_lib_files($i)" >> /mnt/sta_output/sta_worst_min_slack.txt
report_worst_slack -min -digits {4} >> /mnt/sta_output/sta_worst_min_slack.txt

exec echo "$list_of_lib_files($i)" >> /mnt/sta_output/sta_tns.txt
report_tns -digits {4} >> /mnt/sta_output/sta_tns.txt

exec echo "$list_of_lib_files($i)" >> /mnt/sta_output/sta_wns.txt
report_wns -digits {4} >> /mnt/sta_output/sta_wns.txt
}
```

## SDC File ```vsdbabysoc_synthesis.sdc``` :

```
set PERIOD 9.05
set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD
set_clock_uncertainty [expr 0.05 * $PERIOD] -setup [get_clocks clk]
set_clock_uncertainty [expr 0.08 * $PERIOD] -hold [get_clocks clk]
set_clock_transition [expr 0.05 * $PERIOD] [get_clocks clk]


set_input_transition [expr $PERIOD * 0.08] [get_ports ENB_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENB_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```


## Table

Below is the table listing different slack timings (Total Negative Slack, Worst Negative Slack, Worst Setup Slack and Worst Hold Slack) for the listed library files.

| PVT Corner                      | Total Negative Slack           | Worst Negative Slack       | Worst Setup Slack | Worst Hold Slack |
| ----------------------------------| ------------- | ----------| ----------------- | ---------------- |
| sky130_fd_sc_hd__tt_025C_1v80.lib | -8981.5078    | -10.0227  | -10.0227          | -0.3797          |
| sky130_fd_sc_hd__tt_100C_1v80.lib | -7943.0352    | -8.7653   | -8.7653           | -0.3815          |
| sky130_fd_sc_hd__ff_100C_1v65.lib | -4767.1372    | -6.1226   | -6.1226           | -0.442           |
| sky130_fd_sc_hd__ff_100C_1v95.lib | -1683.0513    | -2.8678   | -2.8678           | -0.5001          |
| sky130_fd_sc_hd__ff_n40C_1v56.lib | -10473.207    | -12.0194  | -12.0194          | -0.3826          |
| sky130_fd_sc_hd__ff_n40C_1v65.lib | -7342.2397    | -9.0245   | -9.0245           | -0.422           |
| sky130_fd_sc_hd__ff_n40C_1v76.lib | -4981.9219    | -6.3651   | -6.3651           | -0.4569          |
| sky130_fd_sc_hd__ff_n40C_1v95.lib | -2339.7185    | -3.5006   | -3.5006           | -0.4991          |
| sky130_fd_sc_hd__ss_100C_1v40.lib | -44110.1016   | -37.0507  | -37.0507          | 0.2804           |
| sky130_fd_sc_hd__ss_100C_1v60.lib | -27036.1855   | -23.6857  | -23.6857          | 0.0049           |
| sky130_fd_sc_hd__ss_n40C_1v28.lib | -162572.1875  | -129.3335 | -129.3335         | 1.1813           |
| sky130_fd_sc_hd__ss_n40C_1v35.lib | -108107.0234  | -87.5788  | -87.5788          | 0.715            |
| sky130_fd_sc_hd__ss_n40C_1v40.lib | -86088.9844   | -70.3175  | -70.3175          | 0.4924           |
| sky130_fd_sc_hd__ss_n40C_1v44.lib | -75343.6719   | -62.1922  | -62.1922          | 0.3552           |
| sky130_fd_sc_hd__ss_n40C_1v60.lib | -41920.6055   | -36.2671  | -36.2671          | 0.0262           |
| sky130_fd_sc_hd__ss_n40C_1v76.lib | -28783.7734   | -25.9217  | -25.9217          | -0.184           |

As we can infer from the table above, worst negative slack and worst setup slack are the same.

## Screenshots of Waveforms:

**Note: The timings marked across the Y-axis are in nanoseconds**

### Worst Setup Slack:

![Worst Setup Slack](https://github.com/user-attachments/assets/7daa0093-766e-438e-9b8a-751e8dfde7d7)

### Worst Hold Slack:

![Worst Hold Slack](https://github.com/user-attachments/assets/7479da70-21ad-4f66-93a2-bb2a85712002)

### Total Negative Slack:

![TNS](https://github.com/user-attachments/assets/44ac5f56-cef7-4557-8971-178f387c4965)


## Conclusion:

From the above analysis we can conclude that 

1. ```sky130_fd_sc_hd__ss_n40C_1v28.lib``` Library file has the worst setup slack
2. ```sky130_fd_sc_hd__ff_100C_1v95.lib``` Library file has the worst hold slack
