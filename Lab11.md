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

| PVT Corner                  | TNS         | WNS       | Worst Max Slack | Worst Min Slack |
|--------------------------------|-------------|-----------|-----------------|-----------------|
| sky130_fd_sc_hd__tt_025C_1v80.lib | -16.192     | -0.4633   | -0.4633         | -0.4144         |
| sky130_fd_sc_hd__tt_100C_1v80.lib | -9.4742     | -0.309    | -0.309          | -0.4095         |
| sky130_fd_sc_hd__ff_100C_1v65.lib | 0           | 0         | 1.652           | -0.4749         |
| sky130_fd_sc_hd__ff_100C_1v95.lib | 0           | 0         | 3.1643          | -0.528          |
| sky130_fd_sc_hd__ff_n40C_1v56.lib | -2.9926     | -0.1244   | -0.1244         | -0.4325         |
| sky130_fd_sc_hd__ff_n40C_1v65.lib | 0           | 0         | 1.0087          | -0.4689         |
| sky130_fd_sc_hd__ff_n40C_1v76.lib | 0           | 0         | 2.0276          | -0.4997         |
| sky130_fd_sc_hd__ff_n40C_1v95.lib | 0           | 0         | 3.1894          | -0.5365         |
| sky130_fd_sc_hd__ss_100C_1v40.lib | -3455.4934  | -19.5241  | -19.5241        | 0.1813          |
| sky130_fd_sc_hd__ss_100C_1v60.lib | -1449.187   | -10.2765  | -10.2765        | -0.082          |
| sky130_fd_sc_hd__ss_n40C_1v28.lib | -15358.8008 | -64.9657  | -64.9657        | 1.1056          |
| sky130_fd_sc_hd__ss_n40C_1v35.lib | -9297.9424  | -42.0995  | -42.0995        | 0.6235          |
| sky130_fd_sc_hd__ss_n40C_1v40.lib | -6662.5122  | -32.0962  | -32.0962        | 0.4009          |
| sky130_fd_sc_hd__ss_n40C_1v44.lib | -5188.7397  | -26.3104  | -26.3104        | 0.2669          |
| sky130_fd_sc_hd__ss_n40C_1v60.lib | -1991.1907  | -13.0115  | -13.0115        | -0.0612         |
| sky130_fd_sc_hd__ss_n40C_1v76.lib | -817.7378   | -6.7844   | -6.7844         | -0.2202         |


## Terminal Screenshot for the commands:

![image](https://github.com/user-attachments/assets/1ee6133e-36c4-48a1-9bec-e76b81865356)

## Screenshots of Waveforms:

**Note: The timings marked across the Y-axis are in nanoseconds**

![slack_plot_1](https://github.com/user-attachments/assets/b6d28050-ac32-49de-ac66-97f3807cedec)

![slack_plot_2](https://github.com/user-attachments/assets/de69d894-9bab-4a02-8f3e-0fffffc63f21)

![slack_plot_3](https://github.com/user-attachments/assets/cc614a35-96b5-4d6a-b1aa-a635be4d635e)

![slack_plot_4](https://github.com/user-attachments/assets/bb63a18c-7aec-4c1f-9ca0-efd948ec4861)

## Conclusion:

From the above analysis we can conclude that 

1. ```sky130_fd_sc_hd__ss_n40C_1v28.lib``` Library file has the worst setup slack
2. ```sky130_fd_sc_hd__ff_100C_1v95.lib``` Library file has the worst hold slack
