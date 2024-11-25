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





Makerfile Screenshots:

![image](https://github.com/user-attachments/assets/2f05afd2-1e52-456d-8012-68af2a91ee4a)

![image](https://github.com/user-attachments/assets/eb6df4ad-5726-4f65-a2de-10f81308a34a)

![image](https://github.com/user-attachments/assets/e4a11418-43a2-481b-94ab-7d2047ca256a)

![image](https://github.com/user-attachments/assets/8cf3486a-a73b-4ce2-87ba-a1028d150838)

![image](https://github.com/user-attachments/assets/84469488-e4e5-4856-ab0a-52d7a157b7aa)

![image](https://github.com/user-attachments/assets/fb2104b9-6f69-4906-8acb-297e060f226a)

![image](https://github.com/user-attachments/assets/e16635c0-56ff-4656-baa0-b513d7f54b33)

![image](https://github.com/user-attachments/assets/daaa0619-485f-45aa-b795-ea9a7a4d01fd)


Terminal Command and Screenshots for Synthesis:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

![image](https://github.com/user-attachments/assets/3009fc6a-2d35-44eb-bcf7-a6763c1f2bab)

Synthesis Netlist:

![image](https://github.com/user-attachments/assets/4dc001e3-f44d-483f-ae1a-f7ae75ecd07e)

Synthesis Log:

![image](https://github.com/user-attachments/assets/7f6f1da3-96d3-4a8a-a3e2-1110f891addd)

Synthesis Check:

![image](https://github.com/user-attachments/assets/674065b7-bd3b-46c3-b1d8-07e77700d1ad)

Synthesis Stats:

![image](https://github.com/user-attachments/assets/d08ac606-6b03-4753-b1f4-da5d01acff01)

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

![image](https://github.com/user-attachments/assets/664fa2f1-ae31-4878-9a39-b320b29d6da7)

![image](https://github.com/user-attachments/assets/34b0c0b5-8d98-44bc-8e2f-340a76da036d)
