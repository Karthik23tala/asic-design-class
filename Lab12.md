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
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_19-22/results/placement/

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
ngspice sky130_karinv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

Screenshots of ngspice run

![image](https://github.com/user-attachments/assets/ed86ebda-275a-4efe-9416-7dcaa9d37da0)

Screenshot of generated plot

![image](https://github.com/user-attachments/assets/bc17b374-2102-41be-b0c0-28227afbb92e)


* Rise transition time calculation Rise Transition Time = Time taken for output to rise to 80% − Time taken for output to rise to 20%
* 20% of output (3.3V) = 0.66V
* 80% of output (3.3V) = 2.64V

Screenshot for 20% value:

![image](https://github.com/user-attachments/assets/02182314-e31c-4566-b79a-470042622b38)

Screenshot for 80% value:

![image](https://github.com/user-attachments/assets/221b817b-9c27-46c8-b4db-52ab2bd25407)

**Rise Transition Time =  2.2397ns - 2.1790ns = 0.0607ns or 60.7ps**

* Fall Transition Time = Time taken for output to fall to 80% − Time taken for output to fall to 20% 
* 20% of the output (3.3V) = 0.66V
* 80% of the output (3.3V) = 2.64V

Screenshot for 20% value:

![image](https://github.com/user-attachments/assets/b4acf658-07c3-41f9-a3bd-e23ea9cc6978)


Screenshot for 80% value:

![image](https://github.com/user-attachments/assets/93e162a1-f45b-4f52-b02f-5284b08badaa)

**Fall Transition Time = 4.0935ns - 4.0505ns = 0.043ns or 43ps**

* Rise Cell Delay
Calculation Rise cell delay = Time taken by output to rise to 50% − Time taken by input to fall to 50% 
* 50 % of the output (3.3V) = 1.65V

Screenshot for 50% value:

![image](https://github.com/user-attachments/assets/f3576fe0-0ad9-4218-967b-f45cdce4b0a0)

**Rise Cell Delay = 2.2066ns - 2.1487ns = 0.0579 or 57.9ps**

* Fall Cell Delay
Calculation Fall cell delay = Time taken by output to fall to 50% − Time taken by input to rise to 50% 
* 50 % of the output (3.3V) = 1.65V

Screenshot for 50% value:

![image](https://github.com/user-attachments/assets/6e9d8557-b679-4065-b335-c41e65bacd3b)

**Fall Cell Delay = 4.0751ns - 4.0498ns = 0.0253ns or 25.3ps**

6.Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: 
```
https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html
```

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

Terminal Screenshots for the above commands

![image](https://github.com/user-attachments/assets/0af67b2c-4fad-4b61-9221-e43817186278)

Screenshot of .magicrc file 

![image](https://github.com/user-attachments/assets/2fb439c6-f022-49b4-a811-ae64c9ca1300)

Incorrectly implemented poly.9 simple rule correction

Screenshot of poly rules

![image](https://github.com/user-attachments/assets/792a8dac-4e6e-48da-bcb5-dcb4956bd927)

Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u

![image](https://github.com/user-attachments/assets/d3fb46b5-f512-4702-aa3f-b17aef2f73f1)

![image](https://github.com/user-attachments/assets/1806f7c4-2515-4c28-9e50-3c412464b40d)

Below are the commands (highlighted in the screenshot) that need to be inserted in sky130A.tech file to update drc and fix this error

![image](https://github.com/user-attachments/assets/9a4b528e-d2a9-4af2-a7a0-a61ba46f634e)

![image](https://github.com/user-attachments/assets/96792114-34ca-4f7c-b2a0-5c1705958299)

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

![image](https://github.com/user-attachments/assets/27812378-5d66-4969-984c-643d7e810c42)

Incorrectly implemented difftap.2 simple rule correction



Screenshot of difftap rules

![image](https://github.com/user-attachments/assets/365365d2-52d2-427b-9d3c-8be6bbf2be1f)

Incorrectly implemented in tkcon window

![image](https://github.com/user-attachments/assets/b235386e-aa46-49cc-8b74-dbcfabeb9f2e)


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

![image](https://github.com/user-attachments/assets/deaff09a-d890-4403-a293-5be3e9993a41)

## Day4: Pre-Layout timing analysis and Importance of good clock tree:
<br>

## Final steps for RTL2GDS using tritonRoute and openSTA:
1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.

Conditions to be verified before moving forward with custom designed cell layout:
```
    Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.
    Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch.
    Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.
```

Commands to open the custom inverter layout
```
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_karinv.mag &
```

Screenshot of tracks.info of sky130_fd_sc_hd

![image](https://github.com/user-attachments/assets/6118c125-c458-4baa-bfa7-04030b91d857)


Commands for tkcon window to set grid as tracks of locali layer
```
# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um
```
Screenshot of commands run

![image](https://github.com/user-attachments/assets/f2946451-bb54-4851-9195-f17d4b25aa4a)

Condition 1 verified

![image](https://github.com/user-attachments/assets/78813ac8-f840-44f7-8d8d-1375ae402616)

Condition 2 verified

![image](https://github.com/user-attachments/assets/2c931cc0-c265-4a2a-a385-2ddb34420b79)

Condition 3 verified

![image](https://github.com/user-attachments/assets/da21e84a-03bd-4547-aff8-d44c69625257)

2. Save the finalized layout with custom name and open it.

Command for tkcon window to save the layout with custom name
```
# Command to save as
save sky130_karinv.mag

Command to open the newly saved layout

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_karinv.mag &
```

Screenshot of newly saved layout

![image](https://github.com/user-attachments/assets/4e83a49a-472a-4217-9caa-7ba027914cba)

3. Generate lef from the layout.

Command for tkcon window to write lef
```
# lef command
lef write
```
Screenshot of command run

![image](https://github.com/user-attachments/assets/2c629971-cca0-46f4-8f08-27de5bac7641)

Screenshot of newly created lef file

![image](https://github.com/user-attachments/assets/e28e8e2c-1116-4265-aaaa-195c3df44302)

4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

Commands to copy necessary files to 'picorv32a' design 'src' directory

```
# Copy lef file
cp sky130_karinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```
Screenshot of commands run

![image](https://github.com/user-attachments/assets/34b66ac9-a751-4734-b969-f08bcd5f3000)

![image](https://github.com/user-attachments/assets/e836820f-fa03-4daa-8d4e-d7ecdd4640b1)


5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

Commands to be added to config.tcl to include our custom cell in the openlane flow
```
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```

Edited config.tcl to include the added lef and change library to ones we added in src directory

![image](https://github.com/user-attachments/assets/ffdd8cf6-8145-48c1-8512-9bbd572560f2)

6. Run openlane flow synthesis with newly inserted custom inverter cell.
```
Commands to invoke the OpenLANE flow include new lef and perform synthesis

# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Terminal Screenshots for the above commands

![image](https://github.com/user-attachments/assets/35a39901-3508-4819-8fab-facee4f036a6)

![image](https://github.com/user-attachments/assets/1a712ed5-5ba7-41f5-a6f2-3691e3718c07)

![image](https://github.com/user-attachments/assets/217b47d3-859b-4e73-ba99-7896fa4a3ac2)

![image](https://github.com/user-attachments/assets/05a1f3ad-c9e9-4da5-86e3-31be2c86681c)


7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.
Noting down current design values generated before modifying parameters to improve timing

![image](https://github.com/user-attachments/assets/98905dab-4d9d-4623-8cf2-e2668132cd99)

![image](https://github.com/user-attachments/assets/d6070154-a9b3-4a34-a0fe-35d7a179e715)

Commands to view and change parameters to improve timing and run synthesis
```
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 13-11_15-13 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Screenshot of merged.lef in tmp directory with our custom inverter ```karinv``` as macro

![image](https://github.com/user-attachments/assets/b8f2117c-fc63-4417-9d89-6612c7e52222)

Terminal Screenshots for the above commands

![image](https://github.com/user-attachments/assets/27ee0a7c-46eb-4514-95c1-643250735638)

Comparing to previously noted run values area has increased and worst negative slack has become 0

![image](https://github.com/user-attachments/assets/2dbde6c1-606e-4d87-9571-778e83d58641)

![image](https://github.com/user-attachments/assets/dd5b8627-be26-423d-8205-5d92800d4f55)

8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command
```
# Now we can run floorplan
run_floorplan
```

In case of the above command throwing any unexpected error we can use the following series of commands one by one which is essentially same as that of running ```run_floorplan``` command.

```
init_floorplan
place_io
tap_decap_or
```

Terminal Screenshots for the above commands

![image](https://github.com/user-attachments/assets/e3816ca7-93e6-4daf-a51f-ab2dd62098e2)

Now that floorplan is done we can do placement using following command
```
# Now we are ready to run placement
run_placement
```


Terminal Screenshots for the above commands

![image](https://github.com/user-attachments/assets/fa2f9b62-aba3-485d-8f39-c779d41ebb1b)

![image](https://github.com/user-attachments/assets/54bcb9d4-0d92-47a8-b1c1-31c19a42a40d)


Commands to load placement def in magic in another terminal
```
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_15-13/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshot of placement def in magic

![image](https://github.com/user-attachments/assets/7abd2535-e12d-4e38-883b-16af3c11ed1a)


Screenshot of custom inverter ```karinv``` inserted in placement def with proper abutment

![image](https://github.com/user-attachments/assets/894820b6-77a4-4fe0-b76e-ee9ae1e2f191)

9. Do Post-Synthesis timing analysis with OpenSTA tool.

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing

Commands to invoke the OpenLANE flow include new lef and perform synthesis
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

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Terminal screenshots

![image](https://github.com/user-attachments/assets/a3bcb593-2d2b-4e1f-982d-9e35e9bdcc89)

![image](https://github.com/user-attachments/assets/8686ac91-5a4a-4003-9c66-6bd99d86e58d)

![image](https://github.com/user-attachments/assets/d89d9431-5c63-487a-a528-26e9db59bfdb)

Newly created pre_sta.conf for STA analysis in openlane directory

![image](https://github.com/user-attachments/assets/1b7140ea-2169-4d3c-9cad-7ec876e31835)

Newly created my_base.sdc for STA analysis in openlane/designs/picorv32a/src directory based on the file openlane/scripts/base.sdc

![image](https://github.com/user-attachments/assets/1af7de15-7600-4a8f-8a3a-d371c63c3915)

Commands to run STA in another terminal
```
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```
Terminal Screenshots for the above commands

![image](https://github.com/user-attachments/assets/49bffa18-3e9c-42f7-840d-022045ddc82a)

![image](https://github.com/user-attachments/assets/c0dd04dd-b13c-4683-91eb-113303561e5c)

![image](https://github.com/user-attachments/assets/79c14699-8a39-4a7d-b1e3-6f26273384ae)

![image](https://github.com/user-attachments/assets/32922fa1-2b06-4208-85f9-018f9edce00b)

Since more fanout is causing more delay we can add parameter to reduce fanout and do synthesis again

Commands to include new lef and perform synthesis
```
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 13-11_17-46 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
Terminal Screenshots

![image](https://github.com/user-attachments/assets/462af278-45e3-439b-b936-5f2b81acfd05)

![image](https://github.com/user-attachments/assets/e2ec6e43-4cdc-47a4-9d12-532e627b7fd0)


![image](https://github.com/user-attachments/assets/5900dfa3-eea3-4307-a98f-50371a481fd8)


10. Make timing ECO fixes to remove all violations.

OR gate of drive strength 2 is driving 4 fanouts



Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```
# Reports all the connections to a net
report_net -connections _11672_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced



OR gate of drive strength 2 is driving 4 fanouts


Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
Result - slack reduced


OR gate of drive strength 2 driving OA gate has more delay


Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```
# Reports all the connections to a net
report_net -connections _11643_

# Replacing cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
Result - slack reduced


OR gate of drive strength 2 driving OA gate has more delay

Screenshot from 2024-03-26 10-32-27



Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```
# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
Result - slack reduced



Commands to verify instance _14506_ is replaced with sky130_fd_sc_hd__or4_4
```
# Generating custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_
```
Screenshot of replaced instance


11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Now to insert this updated netlist to PnR flow and we can use write_verilog and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist

Commands to make copy of netlist
```
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_17-46/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```

Screenshot of commands run


Commands to write verilog
```
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_17-46/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```

Screenshot of commands run


Verified that the netlist is overwritten by checking that instance _14506_ is replaced with sky130_fd_sc_hd__or4_4



Since we confirmed that netlist is replaced and will be loaded in PnR but since we want to follow up on the earlier 0 violation design we are continuing with the clean design to further stages

Commands load the design and run necessary stages
```
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 13-11_17-46 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts
```

Terminal Screenshots for the above commands




12. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD
```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_17-46/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_17-46/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_17-46/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
Terminal Screenshots for the above commands and timing report generated



13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis after changing CTS_CLK_BUFFER_LIST
```
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/13-11_17-46/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_17-46/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_17-46/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_17-46/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```
Terminal Screenshots for the above commands and timing report generated



</details>



 <details>
  <summary>Day5 </summary>

##  Final steps for RTL2GDS using tritonRoute and openSTA 

1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Commands to perform all necessary stages up until now
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

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Following commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts

# Now that CTS is done we can do power distribution network
gen_pdn 
```

Screenshots of power distribution network run


Commands to load PDN def in magic in another terminal
```
# Change directory to path containing generated PDN def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_21-01/tmp/floorplan/

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
Screenshots of PDN def


2. Perfrom detailed routing using TritonRoute and explore the routed layout.

Command to perform routing
```
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```
Screenshots of routing run



Commands to load routed def in magic in another terminal
```
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_21-01/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```
Screenshots of routed def



Screenshot of fast route guide present in openlane/designs/picorv32a/runs/13-11_21-01/tmp/routing directory


3. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD
```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_21-01/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_21-01/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_21-01/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/13-11_21-01/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Terminal Screenshots for the above commands and timing report generated


