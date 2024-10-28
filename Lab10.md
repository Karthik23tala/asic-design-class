# Lab 10 Report (24/10/24) - Static Timing Analysis (STA) for Synthesized RISCV Core using OpenSTA
<br>

## Static Timing Analysis (STA):

### Introduction to STA: 

Static Timing Analysis (STA) is a crucial method used in digital circuit design to verify timing constraints without requiring dynamic simulation.

* **Timing Paths**: Sequences between flip-flops or between flip-flops and I/O ports where signal timing requirements must be met.
* **Setup Timing**: Ensures the data signal is stable before the clock edge.
* **Hold Timing**: Ensures the data signal remains stable for a defined period after the clock edge.
* **Clock Constraints**: Important for maintaining synchronization, preventing timing violations across paths, and setting limits on clock skew and jitter.
* **Worst-Case Scenarios**: STA considers maximum delays and factors such as process, voltage, and temperature variations that may impact timing.
* **Tools**: Popular STA tools, such as Synopsys PrimeTime, Cadence Tempus, and ANSYS PathFinder, provide designers with insights into these timing metrics, allowing them to ensure reliable circuit operation before fabrication.

### Why Static Timing Analysis (STA) is required?

Static Timing Analysis (STA) performs several key functions in digital circuit design:

* **Timing Validation**: STA ensures that data signals meet specific timing constraints, allowing stable and accurate output generation.
* **Violation Detection**: It identifies setup and hold violations, preventing issues that could disrupt flip-flop operation and impact sequential logic reliability.
* **Circuit Optimization**: STA pinpoints performance bottlenecks, enabling designers to improve timing through gate sizing, layout adjustments, or optimized clocking strategies.
* **Early Issue Resolution**: STA enables early detection of timing issues, minimizing costly design iterations and reducing risks before the layout or fabrication stages.
* **Power Efficiency**: By optimizing clock frequencies, STA helps balance performance with power consumption for efficient designs.
* **Design Validation**: STA confirms compliance with design specifications, ensuring circuits meet intended operational standards.
* **Process Efficiency**: Automated tools streamline timing validation in complex designs, offering faster insights than traditional simulation.
* **Manufacturing Readiness**: STA considers process, voltage, and temperature variations to ensure performance consistency across real-world conditions.

In summary, these capabilities make STA essential for developing reliable, high-performance digital circuits.

## Register to Register Path:

A **reg-to-reg (register-to-register) path** in digital circuit design is a timing path that starts at the output of one register (or flip-flop) and ends at the input of another register. It represents the time required for a signal to propagate from one register to the next, passing through combinational logic in between, if present.

### Key Aspects of Reg-to-Reg Paths:
- **Setup and Hold Timing**: The timing of the signal reaching the destination register is critical. The setup time is the minimum time before the clock edge when data must be stable, and the hold time is the minimum time after the clock edge when data must remain stable.
- **Clock Cycle Constraints**: For a reg-to-reg path, the signal propagation delay must fit within a single clock cycle. If the delay exceeds the clock period, it can cause a **setup violation**, meaning data does not reach the destination register in time for the next clock cycle.
- **Combinational Logic Delay**: Any logic elements between the registers contribute to the overall delay, and minimizing this delay through path optimization is often essential to meet timing.
- **Timing Analysis**: STA tools analyze reg-to-reg paths to check for setup and hold violations, which are critical for ensuring stable, synchronized operation of sequential elements.

Reg-to-reg paths are among the most common and important timing paths analyzed in STA, as they directly impact the circuit’s maximum operational clock speed and performance.

## Clock to Register Path:

A **reg-to-reg (register-to-register) path** in digital circuit design is a timing path that starts at the output of one register (or flip-flop) and ends at the input of another register. It represents the time required for a signal to propagate from one register to the next, passing through combinational logic in between, if present.

### Key Aspects of Reg-to-Reg Paths:
- **Setup and Hold Timing**: The timing of the signal reaching the destination register is critical. The setup time is the minimum time before the clock edge when data must be stable, and the hold time is the minimum time after the clock edge when data must remain stable.
- **Clock Cycle Constraints**: For a reg-to-reg path, the signal propagation delay must fit within a single clock cycle. If the delay exceeds the clock period, it can cause a **setup violation**, meaning data does not reach the destination register in time for the next clock cycle.
- **Combinational Logic Delay**: Any logic elements between the registers contribute to the overall delay, and minimizing this delay through path optimization is often essential to meet timing.
- **Timing Analysis**: STA tools analyze reg-to-reg paths to check for setup and hold violations, which are critical for ensuring stable, synchronized operation of sequential elements.

Reg-to-reg paths are among the most common and important timing paths analyzed in STA, as they directly impact the circuit’s maximum operational clock speed and performance.

## Static Timing Analysis (STA) for Synthesized RISCV Core:

Commands:

```
sudo docker run -it -v /home/karthik-talakokkula/Documents/opensta_files/:/mnt opensta /bin/bash
cd /OpenSTA/app

read_liberty /mnt/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog /mnt/vsdbabysoc.v  ; # Ensure this points to the correct netlist file
link_design rvmyth

create_clock -name CLK -period 9.05 [get_ports CLK]
set_clock_uncertainty [expr 0.05 * 9.05] -setup [get_clocks CLK]
set_clock_uncertainty [expr 0.08 * 9.05] -hold [get_clocks CLK]
set_clock_transition [expr 0.05 * 9.05] [get_clocks CLK]
set_input_transition [expr 0.08 * 9.05] [all_inputs]

report_checks -path_delay max
report_checks -path_delay min
```

![image](https://github.com/user-attachments/assets/e6383c54-1fda-4e92-a6d0-3fba7083b73e)

Timing Configurations:

- Clock period: 9.05 ns (nanoseconds)
- Setup uncertainty: 5% of clock period
- Clock transition: 5% of clock period
- Hold uncertainty: 8% of clock period
- Input transition: 8% of clock period

Screenshots representing the timing reports:

![image](https://github.com/user-attachments/assets/8fd9574d-5910-44e1-a9c9-bbd38020ef7d)

![image](https://github.com/user-attachments/assets/0d1c69f3-0def-4321-84e9-b9f0234810c0)
