## Lab12: Advanced Physical Design Using Openlane/Sky130 Wokshop
<br>

## Day 1: Introduction to open-source EDA, OpenLANE and Sky130 PDK

### 1.Run ```picorv32a``` design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis

```
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is ```picorv32a```
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit

```

Terminal Screenshots for the above commands execution:

![image](https://github.com/user-attachments/assets/ca1b43a9-3b0c-4a4c-9c15-359ea91c3192)

![image](https://github.com/user-attachments/assets/73ee1337-eae4-45f9-88ba-235d621c5c2c)

Below screenshot signifies that the generated file on particular date 

![image](https://github.com/user-attachments/assets/d3e47449-0787-4717-864d-00ab6b2337a3)

![image](https://github.com/user-attachments/assets/9e562bc9-ad7e-47a9-b406-6a21fd624f89)

### 2. Calculate the flop ratio.

![image](https://github.com/user-attachments/assets/5ccadd5a-d8d6-4d19-a139-389d0cd56856)

![image](https://github.com/user-attachments/assets/f9848d71-57e6-4804-a06c-0a52f73dd1ce)

#### Calculation of Flop Ratio and DFF % from synthesis statistics report file

Flop Ratio = Number of D Flip-Flops/Total Number of Cells

The percentage of Flop Ratio = Flop Ratio * 100

```
Flop Ratio = 1613/14876 = 0.108429685
    
Percentage of Flip Flops = 0.108429685 ∗ 100 = 10.84296854%
```

## Day 2: Good floorplan versus bad floorplan, and introduction to library cells

### Implementation
1.Run ```picorv32a``` design floorplan using OpenLANE flow and generate necessary outputs.
2.Calculate the die area in microns from the values in floorplan def.
3.Load generated floorplan def in magic tool and explore the floorplan.
4.Run ```picorv32a``` design congestion aware placement using OpenLANE flow and generate necessary outputs.
5.Load generated placement def in magic tool and explore the placement. Area of Die in microns = Die width in microns ∗ Die height in microns

## 1. Run ```picorv32a``` design floorplan using OpenLANE flow and generate necessary outputs

Commands to invoke the OpenLANE flow and perform floorplan
```
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is ```picorv32a```
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```

Screenshots for the above commands:

![image](https://github.com/user-attachments/assets/faa181e1-30bb-4eb6-a3e9-525e17ea7f07)

![image](https://github.com/user-attachments/assets/ac87c794-cdcc-406d-ac83-66a411aa7189)

Floorplan completion Screenshot:

![image](https://github.com/user-attachments/assets/3081765d-223c-4ace-bd83-31431b886f44)

## 2. Calculate the die area in microns from the values in floorplan def.

![image](https://github.com/user-attachments/assets/047a6528-b556-487d-8087-b289c0e30c26)

![image](https://github.com/user-attachments/assets/ed50a178-f6f5-4dd1-b2e3-5554b4587fd3)

![image](https://github.com/user-attachments/assets/41b96d36-0e36-4897-bc54-38033d15fffe)

![image](https://github.com/user-attachments/assets/33bf03ca-a8aa-4199-9e4a-d94f56276b0c)

![image](https://github.com/user-attachments/assets/5c438930-c595-437a-b50a-d2c3740733ad)


According to floorplan def 1000 Unit Distance = 1 micron Die
Die width in unit distance = 660685 − 0 = 660685
Die height in unit distance = 671405 − 0 = 671405
Distance in microns = Value in unit distance / 1000 
Die width in microns = 660685/1000 = 660.685 microns 
Dieheight in microns = 671405/1000 = 671.405 microns 
Area of die in microns = 660.685 ∗ 671.405 = 443587.212425 square microns

## 3. Load generated floorplan def in magic tool and explore the floorplan.

Commands to load floorplan def in magic in another terminal
```
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/12-11_17-52/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Snapshot:

![image](https://github.com/user-attachments/assets/f9dfd537-7d02-4c46-906a-a623bb577f24)


Equidistant placement of ports: 

![image](https://github.com/user-attachments/assets/1800c388-5581-4404-8b30-00f7e1bfd5f9)

Port layer as set through config.tcl 

![image](https://github.com/user-attachments/assets/3db88723-c834-498a-b462-890e98674dd0)

![image](https://github.com/user-attachments/assets/3ef16abc-f3a3-4374-8444-e62555ef4ffc)

Decap Cells and Tap Cells

![image](https://github.com/user-attachments/assets/4b601c5a-4e80-4e47-a77b-fa3cc8273de5)

Diagonally equidistant Tap cells

![image](https://github.com/user-attachments/assets/1cbdacd2-a158-49c2-9e3f-80d4ad2d1616)

Unplaced standard cells at the origin

![image](https://github.com/user-attachments/assets/727d50f5-c300-40a0-b8d8-b45d36491565)


## 4. Run ```picorv32a``` design congestion aware placement using OpenLANE flow and generate necessary outputs. Command to run placement

```
# Congestion aware placement by default
run_placement
```

![image](https://github.com/user-attachments/assets/d115c343-9413-4979-a8c1-35eef98c6470)

5. Load generated placement def in magic tool and explore the placement. Commands to load placement def in magic in another terminal

   
```
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![image](https://github.com/user-attachments/assets/3f6f9760-eb8c-4ddb-9aa9-00431dfa85da)

![image](https://github.com/user-attachments/assets/fd6bcdb0-34a4-4cd7-81ce-1ccf8cab526e)

Standard cells legally placed 

![image](https://github.com/user-attachments/assets/d4f7a58a-5eec-4d9a-8f66-41ce3b8f96cc)

Commands to exit from current run

```
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
## Day3: Design Library Cell Using Magic Layout and Cell characterization:

Tasks:

1.Clone custom inverter standard cell design from github repository: Standard cell design and characterization using OpenLANE flow.
2.Load the custom inverter layout in magic and explore.
3.Spice extraction of inverter in magic.
4.Editing the spice model file for analysis through simulation.
5.Post-layout ngspice simulations.
6.Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

1. Clone custom inverter standard cell design from github repository
```
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

![image](https://github.com/user-attachments/assets/e7648c0f-8630-4c39-b381-db4fcefc2c35)



2.Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![image](https://github.com/user-attachments/assets/668f5167-5beb-4906-8875-65535b10e6a6)

PMOS identified

![image](https://github.com/user-attachments/assets/b41d8d61-c3a0-40dd-a716-c4e9d0f575d4)

NMOS identified

![image](https://github.com/user-attachments/assets/4916d7dd-e65f-4a42-a9c2-ed76f7afc2f4)

Output Y connectivity to PMOS and NMOS drain verified 

![image](https://github.com/user-attachments/assets/46a02a36-4d2d-4799-969a-ea3d51abbaf4)

PMOS source connectivity to VDD (here VPWR) verified

![image](https://github.com/user-attachments/assets/8e681cd2-4593-444d-bf22-ada5378234f5)

NMOS source connectivity to VSS (here VGND) verified

![image](https://github.com/user-attachments/assets/ce71a5b4-92f0-4e5b-a3c0-d462b8311d26)

Deleting necessary layout part to see DRC error

![image](https://github.com/user-attachments/assets/86e62d78-e93f-4e05-ad89-c1538fec790c)

3.Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic
```
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```

Screenshot of tkcon window after running above commands

![image](https://github.com/user-attachments/assets/22509ed8-159d-4940-90f8-f765decf91fc)


Screenshot of the Spice file creation and contents:

![image](https://github.com/user-attachments/assets/f0833d32-d6c8-4211-8c63-a2ac58a19140)

![image](https://github.com/user-attachments/assets/d9e55326-0c7a-48d3-8bf8-27f3049419ab)

4.Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

![image](https://github.com/user-attachments/assets/21688d6f-4def-4e8f-8643-065642059d14)

Final edited spice file ready for ngspice simulation

![image](https://github.com/user-attachments/assets/bd03a205-7973-42ac-89c5-1705139fb923)


5.Post-layout ngspice simulations.

Commands for ngspice simulation
```
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

Screenshots of ngspice run



Screenshot of generated plot
![Screenshot from 2024-11-13 12-59-37](https://github.com/user-attachments/assets/2ae82950-601f-4914-a09e-2f9586d4b

* Rise transition time calculation Rise Transition Time = Time taken for output to rise to 80% − Time taken for output to rise to 20%
* 20% of output (3.3V) = 0.66V 80% of output (3.3V) = 2.64V

20% Screenshots

80% Screenshot 


### Rise Transition Time = 2.2393 - 2.1799 = 0.0594 ns = 59.40 ps

* Fall Transition Time = Time taken for output to fall to 80% − Time taken for output to fall to 20% 
* 20% of output (3.3V) = 0.66V 20% of output (3.3V) = 2.64V

20% Screenshots


80% Screenshot 

### Fall Transition Time = 4.09345 - 4.05088 = 0.04257 ns = 42.57 ps

* Rise Cell Delay Calculation Rise cell delay = Time taken by output to rise to 50% − Time taken by input to fall to 50% 
* 50 % of 3.3V = 1.65V

50% Screenshots

### Rise cell delay = 2.2075 - 2.1489 = 0.0586 ns = 58.60 ps

* Fall Cell Delay Calculation Fall cell delay = Time taken by output to fall to 50% − Time taken by input to rise to 50% 
* 50 % of 3.3V = 1.65V

50% Screenshots

### Fall cell delay = 4.0765−4.05 = 0.0265 ns = 26.50 ps

6.Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections
```
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```

Screenshots of commands run


Screenshot of .magicrc file 

Incorrectly implemented poly.9 simple rule correction

Screenshot of poly rules

Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u


New commands inserted in sky130A.tech file to update drc

Commands to run in tkcon window
```
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

Incorrectly implemented difftap.2 simple rule correction

Screenshot of difftap rules



Screenshot of difftap rules

Incorrectly implemented


Commands to run in tkcon window
```
# Loading updated tech file
tech load sky130A.tech

# Change drc style to drc full
drc style drc(full)

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented showing no errors found 



## Day4: Pre-Layout timing analysis and Importance of good clock tree:
## Final steps for RTL2GDS using tritonRoute and openSTA:
1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.

Conditions to be verified before moving forward with custom designed cell layout:

    Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.
    Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch.
    Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.

Commands to open the custom inverter layout
```
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of tracks.info of sky130_fd_sc_hd


Commands for tkcon window to set grid as tracks of locali layer
```
# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um
```
Screenshot of commands run


Condition 1 verified





Condition 2 verified
 H o r i z o n t a l   t r a c k   p i t c h = 0.46   u m 
 

 W i d t h   o f   s t a n d a r d   c e l l = 1.38   u m = 0.46 ∗ 3 



 



Condition 3 verified
 V e r t i c a l   t r a c k   p i t c h = 0.34   u m 

  H e i g h t   o f   s t a n d a r d   c e l l = 2.72   u m = 0.34 ∗ 8 


## Day5: Final steps for RTL2GDS using tritonRoute and openSTA
