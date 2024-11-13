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



Port layer as set through config.tcl 




Decap Cells and Tap Cells



Diagonally equidistant Tap cells



Unplaced standard cells at the origin



## 4. Run ```picorv32a``` design congestion aware placement using OpenLANE flow and generate necessary outputs. Command to run placement

```
# Congestion aware placement by default
run_placement
```
![Screenshot from 2024-11-12 01-10-46](https://github.com/user-attachments/assets/f6950154-e31f-4166-9c6a-5f21f96f16f9)

![Screenshot from 2024-11-12 01-11-54](https://github.com/user-attachments/assets/2b6021b3-2ae3-469e-8209-f6f4e5d4dfa8)





5. Load generated placement def in magic tool and explore the placement. Commands to load placement def in magic in another terminal

   
```
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```


![Screenshot from 2024-11-12 01-18-34](https://github.com/user-attachments/assets/0e04adda-bff1-40fa-9955-575da9261751)

![Screenshot from 2024-11-12 01-19-36](https://github.com/user-attachments/assets/9b3bec25-be46-4ce9-852c-6f20393e4960)


Standard cells legally placed 
![Screenshot from 2024-11-12 01-23-40](https://github.com/user-attachments/assets/2393d0ca-16b9-4369-9db4-fd02cff38515)


Commands to exit from current run

```
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```


  </details>






 <details>
  <summary>Day3 </summary>

  ## Design Library Cell Using Magic Layout and Cell characterization:



  </details>







 <details>
  <summary>Day4 </summary>

  ## Pre-Layout timing analysis and Importance of good clock tree:




</details>



 <details>
  <summary>Day5 </summary>

## Final steps for RTL2GDS using tritonRoute and openSTA:


 
</details>
