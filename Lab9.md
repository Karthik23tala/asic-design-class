# Lab 9 Report(22/10/24) - RVMYTH verilog file synthesis using Yosys and Post Sythesis simulation using iverilog and GTKWave
<br>
## Synthesis using Yosys

Commands:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -lib ../lib/avsddac.lib
read_liberty -lib ../lib/avsdpll.lib
read_verilog vsdbabysoc.v
read_verilog rvmyth.v
read_verilog clk_gate.v
synth -top vsdbabysoc
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show vsdbabysoc
write_verilog -noattr vsdbabysoc.synth.v
!gvim vsdbabysoc.synth.v
```

Synthesised Circuit Diagram using standard cells in the library:

![image](https://github.com/user-attachments/assets/157f0550-09f5-435b-b2d9-a76a27858f73)

Generated Netlist Screenshots:

![image](https://github.com/user-attachments/assets/8ffb23fd-e2d8-4f07-ba1c-b6b5bed148de)

![image](https://github.com/user-attachments/assets/688819cf-cdc8-4a2a-a82b-d540f7d055c1)

![image](https://github.com/user-attachments/assets/b8d15e67-6653-4691-a01b-acf84adbe26d)

![image](https://github.com/user-attachments/assets/228aae11-4cfc-4c33-ad2b-5ccc1a0a3c48)

Terminal Screenshots:

![image](https://github.com/user-attachments/assets/faf7d4c7-783c-42a5-b836-9b770283d086)

![image](https://github.com/user-attachments/assets/04719f9a-3f32-4dee-bd03-aa6315d4aa37)

![image](https://github.com/user-attachments/assets/36b54428-904a-49f8-991a-323fcf3d0605)

![image](https://github.com/user-attachments/assets/f560dfac-88c1-4e43-8dcc-7dcfbbfc594e)

## Post Synthesis Similation (GLS):

Commands:

```
cd VSDBabySoC
mkdir -p output/post_synth_sim && iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I src/module/include -I src/module -I src/gls_model src/module/testbench.v && cd output/post_synth_sim && ./post_synth_sim.out
gtkwave post_synth_sim.vcd
```

Generated Waveform post Gate Level Synthesis simulation:

Zoomed out results showing multiple cycles:

![image](https://github.com/user-attachments/assets/9b50a9d0-c918-48ef-88cc-83b2a46a16cb)

Zoomed in results showing the sum of numbers 1 to 9:

![image](https://github.com/user-attachments/assets/4c3b64a0-df18-4fb7-8ce0-ef4a644cddf4)

Waveform Screenshot from Lab9:

![image](https://github.com/user-attachments/assets/ceed17b7-36f4-4bb7-a0ec-f2d3cb9b7647)

Waveform Screenshot from Lab6: 

![image](https://github.com/user-attachments/assets/929aa869-c852-4de1-a3c3-43925a0c504b)

### Conclusion: From the above comparison of waveforms from both the labs we can conclude that the acheived output matches the expectation i.e., the sum of numbers from 1 to 9 which is 45 in decimal or 2D in hexadecimal format.
