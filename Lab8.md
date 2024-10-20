# Lab 8 Sky130 RTL Design Workshop
<br>

## Day 1: Installing the files, running the RTL Design in iverilog & GTKWave and thereby converting into netlist using Yosys synthesizer

iVerilog Simulator Block Diagram:

![image](https://github.com/user-attachments/assets/a70eaf5c-c37f-4995-8634-b02d2b54ac2c)

First we need to create a directory called VLSI under vsd folder and clone the git of sky130 workshop using the below command

Git Link: https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

```
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```


![image](https://github.com/user-attachments/assets/5bcb2199-d052-4f5c-a2a2-0d27480933fe)

![image](https://github.com/user-attachments/assets/c8275deb-9eb4-48e8-aa9d-1a4f318fc9ed)

Now, we can run the verilog file using iverilog simulator and view the output of vcd file in gtkwave.

![image](https://github.com/user-attachments/assets/6ac5d0a1-b9c8-4117-83f2-183b02eda383)

![image](https://github.com/user-attachments/assets/49973b6f-145d-4355-a4fd-f47a76e89b18)

To view the files i.e., good mux code and testbench we can run the following command.

```
gvim tb_good_mux.v -o good_mux.v
```

![image](https://github.com/user-attachments/assets/ecd6ec16-de27-4405-bd28-360fafeaf7e8)

Yosys synthesizer block diagram:

![image](https://github.com/user-attachments/assets/2e5f0ed9-085e-4bad-ba43-cb1077c80898)

![image](https://github.com/user-attachments/assets/5c4dd848-1d0b-42fb-bc45-1dcb0989689d)

Now in the process of converting from the RTL design to netlist includes the following:

Step 1: We write the verilog code for the required RTL design in terms of behaviour of the circuit
Step 2: This behaviourial code need to be implemented using the basic gates such as AND, OR, NAND, NOR, etc. The ```.lib``` consists of all these basic gates for the RTL design implementation
Step 3: Post implementation of the required circuit using the basic gates they need to connected together in order to form the netlist

The point to be noted is that in the ```.lib``` folder we have multiple instances of the same gate. For instance, we can have the slow, medium and comparatively fast version of 2 input AND gate in the library.
The reason for these multiple instances is that we need to adhere to the setup and hold time conditions for the circuit. Depending on the specifications, we need to pick up the right set of gate from the library for the implementation.

The slow instance of a gate will help to handle the HOLD violation and the fast instance of a gate will help with the SETUP violations of the required or given circuit.

![image](https://github.com/user-attachments/assets/17096a7b-fd45-4614-9e4f-29523b3c2145)

![image](https://github.com/user-attachments/assets/04a5bbf4-16ce-42c1-bee1-7bd803cc0c40)

So, implementing the circuit which is optimally fast is the key to the implementation.

Below are the commands involved in RTL design to netlist generation in yosys.

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib      //To read the library
read_verilog good_mux.v                                         //To read the verilog file
synth -top good_mux                                             //To synthesize the design
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib           //To generate the netlist
show                                                            //To show or view the implemented netlist diagram
write_verilog good_mux_netlist.v                                //To write the verilog file into the netlist
!gvim good_mux_netlist.v                                        //To view the implemented netlist verilog code
```

This shows the total number of input, internal and output signals involved in the design. For instance, in the good_mux.v example we have three input signals i0, i1 and sel. Also, there is one output signal y. No internal signals are involved in this design.

![image](https://github.com/user-attachments/assets/fe8a6816-07bf-4a31-a717-f1cc40174ba1)

![image](https://github.com/user-attachments/assets/1cff1564-10f1-46b2-adc8-8f7e6f974a3b)


```
write_verilog good_mux_netlist.v                                //To write the verilog file into the netlist
!gvim good_mux_netlist.v                                        //To view the implemented netlist verilog code
write_verilog -noattr good_mux__netlist.v                       //To write the verilog file into the netlist by removing the attributes for simpler design
```

Earlier netlist:

![image](https://github.com/user-attachments/assets/3f48683b-4175-49fd-83d2-e34c2bad8ad5)

Netlist post ```-noattr``` tag:

![image](https://github.com/user-attachments/assets/4ba8df8f-382c-47e1-ba82-3209e41a77f5)

## Day 2
