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

## Day 2: Hierarchial vs Flat Synthesis, Flop coding styles and optimisation

### Understanding the library files:

In the cloned sky 130 git, we can find the library files. 

Basically library file is the collection of all the basic gates such as AND, OR, NAND, NOR, etc. 

Now, each gate has multiple instances which vary in the specifications such as power, area, cell leakage power and more but the underhood functionality remains the same. A similar comparison for AND gates is shown below.

![image](https://github.com/user-attachments/assets/ce0546b3-3fb3-449a-b356-4253ec87d978)

### Hierarchial vs Flatten Sythesis

In the next step, we generate the netlist for ```multiple_modules.v```.

This verilog file takes a,b and c as input signals and generate y = a*b+c.

Below are the commands and the respective screenshots.

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr multiple_modules_hier.v
show multiple_modules
```

![image](https://github.com/user-attachments/assets/9fb9dc95-a752-4368-be86-00a0251a9c43)

![image](https://github.com/user-attachments/assets/d057a14e-a64e-4822-b0ce-44dd39cd9d97)

In an ideal expected scenario the generated netlist should have one OR gate and one AND gate. But from the below generated netlist we can see that it has instantiated two sub modules u1 and u2. 

![image](https://github.com/user-attachments/assets/fd27a101-b482-4c6a-bd4a-2fb847dfeacc)

Now when the design has multiple modules then there is another way to generate the netlist which is called as flatten. In flatten, all the submodules are merged into single level and thereby the respective connections are explicitly defined.

Below are the commands and the screenshot of the generated netlist.

```
flatten
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr multiple_modules_flat.v
show multiple_modules
```

![image](https://github.com/user-attachments/assets/db3afac2-327e-4c29-9793-e84f4518814b)

### Flops Coding Styles and Optimisation

In general, during the implementation of combinational circuit which involves multiple levels of gates due to the difference in the propogation delays of the gates we can encounter an unwanted glitch in the output. 

To avoid this, flops are used in between any two combinational circuit stages. Since flip flop is a sequential circuit it activates only at the edge of the clock (posedge or negedge) and thereby provide a stable input to the next stage of the combinational circuit eliminating the glitches in the circuit.

The flip flop can be coded in different styles either asynchronous or synchronous, including the reset or set pin in the circuit.

#### D-Flip flop with Asynchronous Reset

For example, we have run the D-Flip flop with asynchronous reset pin. The moment reset pin is high the output will be reset to zero irrespective of the clock posedge or negedge. Thus, it is the asynchronous reset pin. 

Below are the commands and the respective screenshots:

```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

![image](https://github.com/user-attachments/assets/edc58268-5628-4094-93cd-8e17dfe65bed)

#### D-Flip flop with Asynchronous Set

Similarly, we can run the D-Flip flop with asynchronous set pin. The moment set pin is high the output will be reset to high irrespective of the clock posedge or negedge. Thus, it is the asynchronous set pin. 

```
iverilog dff_asyncres.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

![image](https://github.com/user-attachments/assets/6d061130-fce5-4b75-b561-fd6afdb05438)

#### D-Flip flop with Synchronous Set

In a similar way, we can run the D-Flip flop with synchronous reset pin. In this case, when the reset pin is high the output resets to low after the positive edge of the clock. Here the set pin is dependent on the clock, hence synchronous.

```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

![image](https://github.com/user-attachments/assets/359afb3e-231b-47d8-8641-7fd6775fe31e)

Now, in the next step, we will generate the netlist for all these three types of D-Flip flops using the yosys synthesizer.

1. D-Flip flop with Asynchronous Reset

Commands and Screenshots:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_asyncres_netlist.v
!gvim dff_asyncres_netlist.v
```
![image](https://github.com/user-attachments/assets/1cf6c1ba-6c0f-49ac-9732-b58247fde079)

![image](https://github.com/user-attachments/assets/96793882-54d8-4ae0-be2e-b74c19c57d9e)

![image](https://github.com/user-attachments/assets/7a06381b-d662-45b4-ae34-e2450591df94)

![image](https://github.com/user-attachments/assets/6189a05a-54e4-4cf5-9b4e-ad52c143e545)


2. D-Flip flop with Asynchronous Set

Commands and Screenshots:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_async_set_netlist.v
!gvim dff_async_set_netlist.v
```
![image](https://github.com/user-attachments/assets/9ee952dd-c4bd-4d7d-86bf-f6f1507755a0)

![image](https://github.com/user-attachments/assets/056be057-aa91-4b4c-a324-f4d8005b05ab)

![image](https://github.com/user-attachments/assets/07c0677c-1455-41c1-a87a-48f274a7b4b5)

![image](https://github.com/user-attachments/assets/312dfd70-4903-449c-a1be-9047d5b73c61)

3. D-Flip flop with Synchronous Reset

Commands and Screenshots:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_syncres_netlist.v
!gvim dff_syncres_netlist.v
```

![image](https://github.com/user-attachments/assets/c8f4f63c-06ae-45ba-82ff-632f7e45b92e)

![image](https://github.com/user-attachments/assets/6668896f-8d9a-44eb-bbc8-c5dff604c3df)

![image](https://github.com/user-attachments/assets/4a7888d2-0467-45eb-8c46-7885fb43c4ed)

![image](https://github.com/user-attachments/assets/e75c5be6-4a05-4e94-966e-97b12df6a9c3)

Generating the netlist for the special case circuits.

1. Multiplication by a factor of 2: In this circuit, there are no special hardware multipler blocks are not required for the implementation. Rather we will append zero bit to the LSB of the number effectively shifting it to the left. The same can be seen the netlist images generated from the below commands.

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
show
write_verilog -noattr mul2_netlist.v
!gvim mul2_netlist.v
```

As we can see that there is no hardware involved hence the command of ``` abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib ``` says don't call the ABC function as there is nothing to map.

![image](https://github.com/user-attachments/assets/1956e586-75ee-4c0f-83a4-64ce567a39ed)

![image](https://github.com/user-attachments/assets/865c79a9-9ace-4346-8a61-0f823478be5e)

![image](https://github.com/user-attachments/assets/89b54fc5-690a-463d-a2f4-7a8c8ff1353d)

![image](https://github.com/user-attachments/assets/87a23eaa-1f2c-4519-9b36-3e37550d97fe)

2. Multiplication by a factor of 9: Similar to the previous one, here for multiplication of the given number with a factor of 9 will be implemented without the use of any hardware cells, memories, etc.

Below are the commands and the respective screenshots:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_8.v
synth -top mult8
show
write_verilog -noattr mult8_netlist.v
!gvim mult8_netlist.v
```

![image](https://github.com/user-attachments/assets/5bfc6e45-70a4-4a04-b792-0212716fd3a6)

![image](https://github.com/user-attachments/assets/cc030331-5a7e-4b32-aa38-38ed57390b7e)

![image](https://github.com/user-attachments/assets/9cc030cd-8fa7-4fc6-a437-aa069f613da9)

## Day 3: Combinational and Sequential Optimisations

### Combinational Circuits Optimisation

#### ```opt_check.v``` 2 Input AND Gate

To infer a two input AND gate we will run the ```opt_check.v``` file.

```v
module opt_check(input a, input b, output y);
	assign y = a?b:0;
endmodule
```

Below are the commands and the screenshots:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/7a0877b9-bf3c-4673-a989-222e1ab4cfff)

![image](https://github.com/user-attachments/assets/465b32f4-786a-4f04-90a7-bb4f51a7f87c)

#### ```opt_check2.v``` 2 Input OR Gate

Similarly for a two input OR gate, we will run the ```opt_check2.v``` file.

```v
module opt_check2(input a, input b, output y);
	assign y = a?1:b;
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_check2
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/43e2dcbb-87ed-49fa-bc64-e08892409572)

![image](https://github.com/user-attachments/assets/b955898d-c5b2-4c2f-ab63-aaa6a8aa7193)


#### ```opt_check3.v``` 3 Input AND Gate

The ```opt_check3.v``` file infers three input AND gate

```v
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v
synth -top opt_check3
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/4ac0650b-ce4c-4260-8e3a-b0c645d35730)

![image](https://github.com/user-attachments/assets/f66a00c1-c289-4d0c-9dfb-ca20af77d91a)

#### ```opt_check4.v``` XNOR Gate

```v
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```

Below are the commands and respective screenshots:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check4.v
synth -top opt_check4
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/4067ff86-2f07-401a-b69d-8e077b4b3ec0)

![image](https://github.com/user-attachments/assets/caef6a1c-1fd6-4590-a3ad-dbce2c20fa11)

#### Multiple modules optimisation 1

```v
module sub_module1(input a, input b, output y);
	assign y = a & b;
endmodule

module sub_module2 (input a, input b output y);
	assign y = a^b;
endmodule

module multiple_module_opt(input a, input b input c, input d output y);
	wire n1,n2, n3;

	sub_module1 U1 (.a(a), .b(1'b1), .y(n1));
	sub_module2 U2 (.a(n1), .b(1'b0), .y(n));
	sub_module2 U3 (.a(b), .b(d), .y(n3));

	assign y = c | (b & n1);
endmodule
```

Below are the commands and respective screenshots:

```
yosys
flatten
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt.v
synth -top multiple_module_opt
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_module_opt
```

![image](https://github.com/user-attachments/assets/b1baba70-061d-413e-a261-7b951a1474fd)

![image](https://github.com/user-attachments/assets/883186f6-2169-433b-bcc4-15fe9f88980f)


#### Multiple modules optimisation 2

```v
module sub_module(input a input b output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a, input b input c, input d, output y);
	wire n1,n2, n3;

	sub_module U1 (.a(a), .b(1'b0), y(n));
	sub_module U2 (.a(b), .b(c), .y(n2));
	sub_module U3 (.a(n2), .b(d), .y(n));
	sub_module U4 (.a(n3), .b(n1), .y(y));
endmodule
```

Below are the commands and respective screenshots:

```
yosys
flatten
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt2.v
synth -top multiple_module_opt2
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_module_opt2
```

![image](https://github.com/user-attachments/assets/347175cf-51fd-4e01-bb07-9203dc99d280)

![image](https://github.com/user-attachments/assets/698039b2-305a-4c0d-8d78-4c37124470d3)

### Sequential Circuits Optimisation

#### ```dff_const1.v``` - DFF with active low asynchronous reset

Code:

```v
//Design
module dff_const1(input clk, input reset, output reg q); 
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
//Testbench
module tb_dff_const1; 
	reg clk, reset;
	wire q;

	dff_const1 uut (.clk(clk),.reset(reset),.q(q));

	initial begin
		$dumpfile("tb_dff_const1.vcd");
		$dumpvars(0,tb_dff_const1);
		// Initialize Inputs
		clk = 0;
		reset = 1;
		#3000 $finish;
	end

	always #10 clk = ~clk;
	always #1547 reset=~reset;
endmodule
```

Commands and Screenshots:

```
iverilog dff_const1.v tb_dff_const1.v
./a.out
gtkwave tb_dff_const1.vcd
```

![image](https://github.com/user-attachments/assets/bff136b8-3ade-49b9-8df8-5ed3cbb7cc2b)

In the above waveform, we can see that the output goes low as soon as the reset pin goes high i.e., it does not wait for the next clock edge to change the state of the circuit. Hence, asynchronous reset.

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/1d9f20b8-9f2c-4af0-9406-7b8170f46467)

![image](https://github.com/user-attachments/assets/b7fdac5e-6897-4fa0-bbf2-0336b4cdf199)

#### ```dff_const2.v``` - DFF with active high asynchronous reset

Code:

```v
module dff_const2(input clk, input reset, output reg q); 
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
//Testbench
module tb_dff_const2; 
	reg clk, reset;
	wire q;

	dff_const2 uut (.clk(clk),.reset(reset),.q(q));

	initial begin
		$dumpfile("tb_dff_const1.vcd");
		$dumpvars(0,tb_dff_const1);
		// Initialize Inputs
		clk = 0;
		reset = 1;
		#3000 $finish;
	end

	always #10 clk = ~clk;
	always #1547 reset=~reset;
endmodule
```

Commands and Screenshots:

```
iverilog dff_const2.v tb_dff_const2.v
./a.out
gtkwave tb_dff_const2.vcd
```

Output Waveform:

![image](https://github.com/user-attachments/assets/01ffa7b0-8d5c-4fbf-a788-7f7fb115cb3a)

Now to generate the netlist using yosys:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/aa32d63c-23f8-4c36-9472-6cb4d445d17f)

![image](https://github.com/user-attachments/assets/6216abc4-5a88-41a8-abd6-ac8a0119e67d)

#### ```dff_const3.v``` - DFF with active low asynchronous reset

Code:

```v
module dff_const3(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b1;
			q1 <= 1'b0;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

Commands and Screenshots:

```
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave tb_dff_const3.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```


![image](https://github.com/user-attachments/assets/58f1b67b-b043-4353-9b26-533b305c1143)

![image](https://github.com/user-attachments/assets/2cf9da8d-7b41-4dec-b24a-8c36f5a82c97)

![image](https://github.com/user-attachments/assets/b20e6f69-340d-4512-8be9-bbf97336ab17)


#### ```dff_const4.v``` - DFF with active high asynchronous reset

Code:

```v
module dff_const4(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b1;
			q1 <= 1'b1;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

Commands and Screenshots:

```
iverilog dff_const4.v tb_dff_const4.v
./a.out
gtkwave tb_dff_const4.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const4.v
synth -top dff_const4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/e626aee8-0af9-45e8-872c-544c7c9f2e3b)

![image](https://github.com/user-attachments/assets/ca59404c-1557-4b78-becc-f9a8dc0dfd0c)

![image](https://github.com/user-attachments/assets/f56872ed-6013-4eea-b494-07d7b4eb1fb9)


#### ```dff_const5.v``` - DFF with asynchronous reset

Code:

```v
//Design
module dff_const5(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b0;
			q1 <= 1'b0;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

Commands and Screenshots:

```
iverilog dff_const5.v tb_dff_const5.v
./a.out
gtkwave tb_dff_const5.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const5.v
synth -top dff_const5
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/2dfc8270-1abb-4d3c-999b-76704aebdb0d)

![image](https://github.com/user-attachments/assets/6749a1a4-0c1f-4f64-ad28-9dd705af5593)

![image](https://github.com/user-attachments/assets/b099df41-7701-4d5a-b2a8-639ab91e4079)

#### ```counter_opt.v``` - Counter Optimisation 1

Code:

```v
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```

Commands and Screenshots:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/cd9e0702-3021-4d50-9e26-dbaa63da9d38)

![image](https://github.com/user-attachments/assets/0325ef6d-2fa8-4a81-8b12-404f3d1b63b4)


#### ```counter_opt2.v``` - Counter Optimisation 2

Code:

```v
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```

Commands and Screenshots:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt2.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/user-attachments/assets/0372b41f-0c68-4da0-b9a3-1c8327ac54ec)

![image](https://github.com/user-attachments/assets/1ce01b08-52c4-46f1-b79a-da0f32ba5919)


## Day 4: GLS , Blocking vs Non-blocking and Synthesis-Simulation mismatch

### ```ternary_operator_mux.v``` - 2:1 MUX using ternary operator

Code:

```v
module ternary_operator_mux(input i0, input i1, input sel, output y);
	assign y = sel?i1:i0;
endmodule
```

Commands and Screenshots:

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr ternary_operator_mux_net.v
!gvim ternary_operator_mux_net.v
```

![image](https://github.com/user-attachments/assets/f5f95a85-2083-4a1a-b1dd-bd71e0f34bc1)

![image](https://github.com/user-attachments/assets/ca44afe4-4364-4bf2-945a-817e7c068bd4)

![image](https://github.com/user-attachments/assets/bdc989aa-1af9-4e69-8a65-0ee67a5ccb9b)

![image](https://github.com/user-attachments/assets/584b09c2-a08e-4bb4-b00c-0b92707b8ee8)

Gate-level synthesis commands and screenshots:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

![image](https://github.com/user-attachments/assets/3456bdbb-39fd-495f-a775-574c27d67bfd)

![image](https://github.com/user-attachments/assets/101eceb0-d8c1-481b-8c83-f1f3dfe70c32)

The observation here is that more number of signals are generated by using the gate level synthesis.

### ```bad_mux.v``` - 2:1 MUX with always condition applied only for select line

Code:

```v
module bad_mux(input i0, input i1, input sel, output reg y);
	always@(sel)
	begin
		if(sel)
			y <= i1;
		else
			y <= i0;
	end
endmodule
```

Commands and Screenshots:

```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr bad_mux_net.v
!gvim bad_mux_net.v
```

![image](https://github.com/user-attachments/assets/68ac0899-e514-4170-88c1-48c7ac564ea4)

![image](https://github.com/user-attachments/assets/806920a8-2a6e-4484-b3ed-0f2420698843)

![image](https://github.com/user-attachments/assets/95c29be1-b65f-4538-a28c-f47f1fbd8345)

![image](https://github.com/user-attachments/assets/f2be56db-27e1-4c89-8c89-d1478c1b4a28)

Gate-level synthesis commands and screenshots:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![image](https://github.com/user-attachments/assets/a501bb1e-2c58-4798-b201-db123db951c5)

![image](https://github.com/user-attachments/assets/d6c9bf6a-f451-494c-ae4d-921c9af152df)







