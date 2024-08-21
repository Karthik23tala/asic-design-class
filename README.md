# ASIC Design 

<tr></tr>

This is the GitHub repository for ASIC Design Class Lab Sessions.

<details>
<summary>Lab 1</summary>
<br>

## ASIC Lab 1 Report (16/07/2024)
### Problem Statement
Compile program for sum from 1 to n using c and riscv compiler

#### Task 1: Compile the program in C using GCC

Code:
```c

#include <stdio.h>

int main() {
    int i, n=5, sum=0;
    for(i=1; i<=n; i++){
      sum = sum + i;
    }
    printf("The sum from 1 to %d is %d\n", n, sum);
    return 0;
}
```

Output:

<img width="960" alt="image" src="https://github.com/user-attachments/assets/c82850ee-7320-4c40-b218-3f1710f3dd8e">

#### Task 2: Compile the program in C using RISC-V compiler

Output: For O1


<img width="960" alt="image" src="https://github.com/user-attachments/assets/65a49cb5-4215-4d3b-936e-29cb8d9e4343">


Output: For OFast


<img width="960" alt="image" src="https://github.com/user-attachments/assets/80d9f58b-c041-41d9-bd8b-27b8de803d7b">
</details>

<details>
<summary>Lab 2</summary>
<br>

## ASIC Lab 2 Report (19/07/2024)

### Problem Statement
Compile the C code using Spike Simulator

#### Task: Compile the C code using Spike Simulator instruction by instruction
Code:

```c

#include <stdio.h>

int main() {
    int i, n=45, sum=0;
    for(i=1; i<=n; i++){
      sum = sum + i;
    }
    printf("The sum from 1 to %d is %d\n", n, sum);
    return 0;
}

```

Execute the code file complied by RISCV GCC compiler in the Spike simulator
```
spike pk sum1ton.o
```

![Screenshot from 2024-07-21 22-12-48](https://github.com/user-attachments/assets/132df7a0-50f3-4c89-8cf3-441c0d5f3593)


The code shows the output same as that of C compilation and GCC compilation

We can also compile the code using Spike simulator instruction by instruction with the following command:

```
spike -d pk sum1ton.o
until pc 0 100b0
```

The contents in the register a0 can be viewed with the following command:

```
reg 0 a2
```

![Screenshot from 2024-07-21 22-24-56](https://github.com/user-attachments/assets/d8f6cf0a-b12e-441f-b089-d7f17831ce77)

In the same way, the entire code can be run instruction by instruction by just pressing the "Enter" which moves it to the next command.

Now, in the addition immediate command, below are the contents of sp register before and after the execution.

Contents of sp register before command execution:

![Screenshot from 2024-07-21 22-29-37](https://github.com/user-attachments/assets/cad0ba81-d2ba-446e-bac4-71888e42c777)

Contents of sp register after command execution:

![Screenshot from 2024-07-21 22-30-17](https://github.com/user-attachments/assets/498088dd-0ef8-4dea-8819-5eef236472f8)

</details>

<details>
<summary>Lab 3</summary>
<br>

## ASIC Lab 3 Report (22/07/2024)

### Problem Statement
Decoding the RISCV Instructions and Plotting in GTK Wave

### Task 1: Identifying and Decoding the RISCV Instructions
____________________________________________________________________________________________________________________
```

ADD r0, r1, r2
SUB r2, r0, r1
AND r1, r0, r2
OR r8, r1, r5
XOR r8, r0, r4
SLT r00, r1, r4
ADDI r02, r2, 5
SW r2, r0, 4
SRL r06, r01, r1
BNE r0, r0, 20
BEQ r0, r0, 15
LW r03, r01, 2
SLL r05, r01, r1
 
```

### Decoding the instructions:

R-Type Instructions
Format: funct7 | rs2 | rs1 | funct3 | rd | opcode

ADD r1, r2, r3

* opcode: 0110011 (R-type)
* funct3: 000 (ADD)
* funct7: 0000000
* rd: 00101 (r1)
* rs1: 00010 (r2)
* rs2: 00011 (r3)
* Binary: 0000000 00011 00010 000 00101 0110011
* Hex: 0x000282B3

SUB r3, r1, r2

* opcode: 0110011 (R-type)
* funct3: 000 (SUB)
* funct7: 0100000
* rd: 00011 (r3)
* rs1: 00101 (r1)
* rs2: 00010 (r2)
* Binary: 0100000 00010 00101 000 00011 0110011
* Hex: 0x020292B3

AND r2, r1, r3

* opcode: 0110011 (R-type)
* funct3: 111 (AND)
* funct7: 0000000
* rd: 00010 (r2)
* rs1: 00101 (r1)
* rs2: 00011 (r3)
* Binary: 0000000 00011 00101 111 00010 0110011
* Hex: 0x00032333

OR r8, r2, r5

* opcode: 0110011 (R-type)
* funct3: 110 (OR)
* funct7: 0000000
* rd: 01000 (r8)
* rs1: 00010 (r2)
* rs2: 00101 (r5)
* Binary: 0000000 00101 00010 110 01000 0110011
* Hex: 0x000282B3

XOR r8, r1, r4

* opcode: 0110011 (R-type)
* funct3: 100 (XOR)
* funct7: 0000000
* rd: 01000 (r8)
* rs1: 00101 (r1)
* rs2: 00100 (r4)
* Binary: 0000000 00100 00101 100 01000 0110011
* Hex: 0x00028233

SLT r10, r2, r4

* opcode: 0110011 (R-type)
* funct3: 010 (SLT)
* funct7: 0000000
* rd: 01010 (r10)
* rs1: 00010 (r2)
* rs2: 00100 (r4)
* Binary: 0000000 00100 00010 010 01010 0110011
* Hex: 0x00022233

I-Type Instructions
Format: imm[11:0] | rs1 | funct3 | rd | opcode

ADDI r12, r3, 5

* opcode: 0010011 (I-type)
* funct3: 000 (ADDI)
* imm[11:0]: 0000 0000 0101 (5)
* rd: 01100 (r12)
* rs1: 00011 (r3)
* Binary: 0000 0000 0101 00011 000 01100 0010011
* Hex: 0x00030313

LW r13, r11, 2

* opcode: 0000011 (I-type)
* funct3: 010 (LW)
* imm[11:0]: 0000 0000 0010 (2)
* rd: 01101 (r13)
* rs1: 01011 (r11)
* Binary: 0000 0000 0010 01011 010 01101 0000011
* Hex: 0x000B0313

S-Type Instructions
Format: imm[11:5] | rs2 | rs1 | funct3 | imm[4:0] | opcode

SW r3, r1, 4

* opcode: 0100011 (S-type)
* funct3: 010 (SW)
* imm[11:5]: 0000 0000 (4)
* imm[4:0]: 00100 (4)
* rs1: 00001 (r1)
* rs2: 00011 (r3)
* Binary: 0000 0000 0 00011 00001 010 00100 0100011
* Hex: 0x00030223

B-Type Instructions

Format: imm[12] | imm[10:5] | rs2 | rs1 | funct3 | imm[4:1] | imm[11] | opcode

BNE r0, r1, 20

* opcode: 1100011 (B-type)
* funct3: 001 (BNE)
* imm[12]: 0
* imm[10:5]: 0001010 (20)
* imm[4:1]: 0000
* imm[11]: 0
* rs1: 00001 (r1)
* rs2: 00000 (r0)
* Binary: 0000 0000 0101 0000 001 00000 1100011
* Hex: 0x00A00313

BEQ r0, r0, 15

* opcode: 1100011 (B-type)
* funct3: 000 (BEQ)
* imm[12]: 0
* imm[10:5]: 0000111 (15)
* imm[4:1]: 0000
* imm[11]: 0
* rs1: 00000 (r0)
* rs2: 00000 (r0)
* Binary: 0000 0000 0111 0000 000 00000 1100011
* Hex: 0x00000313

R-Type Instructions with Shifts

Format: funct7 | rs2 | rs1 | funct3 | rd | opcode

SRL r16, r11, r2

* opcode: 0110011 (R-type)
* funct3: 101 (SRL)
* funct7: 0000000
* rd: 10000 (r16)
* rs1: 01011 (r11)
* rs2: 00010 (r2)
* Binary: 0000000 00010 01011 101 10000 0110011
* Hex: 0x000B0233

SLL r15, r11, r2

* opcode: 0110011 (R-type)
* funct3: 001 (SLL)
* funct7: 0000000
* rd: 01111 (r15)
* rs1: 01011 (r11)
* rs2: 00010 (r2)
* Binary: 0000000 00010 01011 001 01111 0110011
* Hex: 0x000B0313

| Instruction         | Type | Binary Instruction                                        | Hex Code   |
|---------------------|------|-----------------------------------------------------------|------------|
| `ADD r1, r2, r3`   | R    | 0000000 00011 00010 000 00101 0110011                    | `0x000282B3` |
| `SUB r3, r1, r2`   | R    | 0100000 00010 00101 000 00011 0110011                    | `0x020292B3` |
| `AND r2, r1, r3`   | R    | 0000000 00011 00101 111 00010 0110011                    | `0x00032333` |
| `OR r8, r2, r5`    | R    | 0000000 00101 00010 110 01000 0110011                    | `0x000282B3` |
| `XOR r8, r1, r4`   | R    | 0000000 00100 00101 100 01000 0110011                    | `0x00028233` |
| `SLT r10, r2, r4`  | R    | 0000000 00100 00010 010 01010 0110011                    | `0x00022233` |
| `ADDI r12, r3, 5`  | I    | 0000 0000 0101 00011 000 01100 0010011                    | `0x00030313` |
| `LW r13, r11, 2`   | I    | 0000 0000 0010 01011 010 01101 0000011                    | `0x000B0313` |
| `SW r3, r1, 4`     | S    | 0000 0000 0 00011 00001 010 00100 0100011                | `0x00030223` |
| `SRL r16, r11, r2` | R    | 0000000 00010 01011 101 10000 0110011                    | `0x000B0233` |
| `BEQ r0, r0, 15`   | B    | 0000 0000 0111 0000 000 00000 1100011                    | `0x00000313` |
| `BNE r0, r1, 20`   | B    | 0000 0000 0101 0000 001 00000 1100011                    | `0x00A00313` |
| `SLL r15, r11, r2` | R    | 0000000 00010 01011 001 01111 0110011                    | `0x000B0313` |

| Instruction  | Type | Opcode | rs2  | rs1  | funct7 | funct3 | rd   | imm                | 32-bit Instruction Code |
|--------------|------|--------|------|------|--------|--------|------|--------------------|--------------------------|
|  `ADD r1, r2, r3`  | R    | 0110011 | 00011 | 00010 | 0000000 | 000   | 00001 | -                  | 0x000282B3 |
|  `SUB r3, r1, r2` | R    | 0110011 | 00010 | 00001 | 0100000 | 000   | 00011 | -                  | 0x400282B3 |
|  `AND r2, r1, r3`  | R    | 0110011 | 00011 | 00001 | 0000000 | 111   | 00010 | -                  | 0x00C30333 |
|  `OR r8, r2, r5`   | R    | 0110011 | 00101 | 00010 | 0000000 | 110   | 01000 | -                  | 0x0002B233 |
|  `XOR r8, r1, r4`  | R    | 0110011 | 00100 | 00001 | 0000000 | 100   | 01000 | -                  | 0x0001B233 |
|  `SLT r10, r2, r4` | R    | 0110011 | 00100 | 00010 | 0000000 | 010   | 01010 | -                  | 0x0002A233 |
|  `ADDI r12, r3, 5` | I    | 0010011 | -    | 00011 | -      | 000   | 01100 | 000000000101       | 0x00530313 |
|  `SW r3, r1, 4`    | S    | 0100011 | 00011 | 00001 | -      | 010   | -    | 000000000100       | 0x00412123 |
|  `SRL r16, r11, r2` | R    | 0110011 | 00010 | 01011 | 0000000 | 101   | 10000 | -                  | 0x000B9313 |
|  `BNE r0, r1, 20`  | B    | 1100011 | -    | 00001 | -      | 001   | -    | 000000000101       | 0x00514163 |
|  `BEQ r0, r0, 15`  | B    | 1100011 | -    | 00000 | -      | 000   | -    | 000000000111       | 0x00700063 |
|  `LW r13, r11, 2`  | I    | 0000011 | -    | 01011 | -      | 010   | 01101 | 000000000010       | 0x002B0323 |
|  `SLL r15, r11, r2` | R    | 0110011 | 00010 | 01011 | 0000000 | 001   | 01111 | -                  | 0x000B5B33 |

Below is the tabulated difference between standard and hardcoded ISA instruction

| Operation            | Standard RISC-V ISA | Standard RISC-V ISA (Binary)                        | Hardcoded ISA | Hardcoded ISA (Binary)                        |
|----------------------|----------------------|------------------------------------------------------|---------------|------------------------------------------------|
| `ADD R6, R2, R1`    | `32'h00110333`       | `000000000001 00010 000 00110 0110011`             | `32'h02208300` | `000000100010 00000 100 00000 01100000`      |
| `SUB R7, R1, R2`    | `32'h402083b3`       | `010000000010 00000 000 00111 0110011`             | `32'h02209380` | `000000100010 01000 100 10000 01100000`      |
| `AND R8, R1, R3`    | `32'h0030f433`       | `000000000011 00000 111 01000 0110011`             | `32'h0230a400` | `000000100011 01000 101 00100 01100000`      |
| `OR R9, R2, R5`     | `32'h005164b3`       | `000000000101 00010 110 01001 0110011`             | `32'h02513480` | `000000100101 00010 110 10100 01100000`      |
| `XOR R10, R1, R4`   | `32'h0040c533`       | `000000000100 00000 100 01010 0110011`             | `32'h0240c500` | `000000100100 00000 100 11000 01100000`      |
| `SLT R1, R2, R4`    | `32'h0045a0b3`       | `000000000100 00010 010 00001 0110011`             | `32'h02415580` | `000000100100 00010 101 01010 01100000`      |
| `ADDI R12, R4, 5`   | `32'h004120b3`       | `000000000101 00010 010 01100 0010011`             | `32'h00520600` | `000000100100 00010 010 00010 01100000`      |
| `BEQ R0, R0, 15`    | `32'h00000f63`       | `000000000000 00000 000 00000 1100011`             | `32'h00f00002` | `000000000000 00000 000 00000 11000000`      |
| `SW R3, R1, 2`      | `32'h0030a123`       | `000000000011 00000 010 00001 0100011`             | `32'h00209181` | `000000100010 00000 010 00001 01100000`      |
| `LW R13, R1, 2`     | `32'h0020a683`       | `000000000010 00000 010 01101 0000011`             | `32'h00208681` | `000000100010 00000 010 01100 01100000`      |
| `SRL R16, R14, R2`  | `32'h0030a123`       | `000000000011 00000 010 00001 0100011`             | `32'h00271803` | `000000100010 00000 011 00110 01100000`      |
| `SLL R15, R1, R2`   | `32'h002097b3`       | `000000000010 00000 111 01111 0110011`             | `32'h00208783` | `000000100010 00000 111 01111 01100000`      |


### Task 2: Plotting instructions in GTK Wave
____________________________________________________________________________________________________________________


Code:

<img width="296" alt="code" src="https://github.com/user-attachments/assets/5e3e80f9-f41e-4769-9632-9ca0d3f74c37">

```
Instruction 1: ADD R6, R2, R1
```

<img width="468" alt="Picture1" src="https://github.com/user-attachments/assets/426c6584-67fd-4f91-8311-e6a729abcf4c">

```
Instruction 2: SUB R7, R1, R2
```

<img width="468" alt="Picture2" src="https://github.com/user-attachments/assets/4d6c6fe7-07f3-4496-81c7-578a687fdd8b">

```
Instruction 3: AND R8, R1, R3
```

<img width="468" alt="Picture3" src="https://github.com/user-attachments/assets/4e7f3035-d4b3-487c-a23b-369752826fea">

```
Instruction 4: OR R9, R2, R5
```

<img width="468" alt="Picture4" src="https://github.com/user-attachments/assets/32fbfad7-8339-43de-a1a0-3e68cba93acd">

```
Instruction 5: XOR R10, R1, R4
```
<img width="468" alt="Picture5" src="https://github.com/user-attachments/assets/a1998c3b-9f58-4b5a-bbb4-73afd8d6b835">

```
Instruction 6: SLT R1, R2, R4
```

<img width="468" alt="Picture6" src="https://github.com/user-attachments/assets/5ae2f304-e38f-4b3b-90b3-62072592410c">

```
Instruction 7: ADDI R12, R4, 5
```

<img width="468" alt="Picture7" src="https://github.com/user-attachments/assets/f27b609d-9eae-4451-90fd-9c5b1c41b66b">

```
Instruction 8: BEQ R0, R0, 15
```

<img width="468" alt="Picture8" src="https://github.com/user-attachments/assets/54217f22-c3a1-4a89-8e03-3c275abe33a3">

```
Instruction 9: BNE R0, R1, 20
```

<img width="468" alt="Picture9" src="https://github.com/user-attachments/assets/c70be765-5704-4145-9a23-de594864d218">

```
Instruction 10: SLL R15, R1, R2
```

<img width="468" alt="Picture10" src="https://github.com/user-attachments/assets/aabc447d-a331-4141-88cb-45a6b68f9134">

</details>


<details>
<summary>Lab 4</summary>
<br>

# ASIC Lab 4 Report (13/08/2024)

## Problem Statement

Compile program for EMI Calculator using C and RISCV compiler

****1 . Compilation in C****

**Step 1**: Write the C program in Leafpad editor in Ubuntu using the following command

```
leafpad emicalculator.c &
```


Code:
```c
#include <stdio.h>
#include <math.h>

// Function to calculate EMI
double calculateEMI(double principal, double annualInterestRate, int tenureInYears) {
    double monthlyInterestRate = annualInterestRate / (12 * 100); 
    // Annual rate to monthly and percentage to fraction
    int tenureInMonths = tenureInYears * 12;
    double EMI;

    EMI = (principal * monthlyInterestRate * pow(1 + monthlyInterestRate, tenureInMonths)) / 
          (pow(1 + monthlyInterestRate, tenureInMonths) - 1);

    return EMI;
}

int main() {
    double principal, annualInterestRate, EMI;
    int tenureInYears;

    // User inputs for principal, interest rate, and tenure
    printf("Enter the principal loan amount: ");
    scanf("%lf", &principal);
    
    printf("Enter the annual interest rate (in percentage): ");
    scanf("%lf", &annualInterestRate);
    
    printf("Enter the tenure of the loan (in years): ");
    scanf("%d", &tenureInYears);

    // Calculate EMI
    EMI = calculateEMI(principal, annualInterestRate, tenureInYears);

    // Display the EMI result
    printf("Your monthly EMI is: %.2lf\n", EMI);

    return 0;
}
```

**Step 2**: Compile the C program using in Compiler using the following command in Ubuntu
```
gcc emicalculator.c -o emicalculator  -lm
```

In this command we have used ```-lm``` to use ```<math.h>``` library file in C.

**Step 3**: Run the C program with the below command
```
./emicalculator
```

The above program takes input of Principal Amount, Annual Rate of Interest and Tenure in years. Post the required inputs, it provides the EMI amount that needs to be paid.

Below is the snapshot of an example for the same.

![1](https://github.com/user-attachments/assets/c0d1bdc7-896f-414f-abeb-e721a5742f68)

****2 . Compilation in RISCV****

**Step 1**: We need to run the same .C file as in the previous case but in the RISCV with the following command
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i emicalculator.c -lm
```

Similar to the previous case, we have used ```-lm``` to use ```<math.h>``` library file in C.

**Step 2**: Run the program in SPIKE simulator with the help of the below command
```
spike pk a.out
```

The above program takes input of Principal Amount, Annual Rate of Interest and Tenure in years. Post the required inputs, it provides the EMI amount that needs to be paid.

Below is the snapshot of an example for the same.

![2](https://github.com/user-attachments/assets/7d07f191-9aff-4853-a6ff-a83bf72848e0)

</details>


<details>
<summary>Lab 5</summary>
<br>

# ASIC Lab 5 Report (15/08/2024)

In this lab, we have performed the lab sessions of Day 3, 4 and 5 from RISC-V based MYTH Workshop.

## Combinational Circuits Implementation

Makerchip is an online IDE in which we can write the code in TL Verilog and simulate to observe the diagram, waveforms and also the visualization.

Below is the snapshot of the IDE.

![image](https://github.com/user-attachments/assets/ce3e31a1-2c05-49b8-938f-887e086d20ec)

### Inverter

The simplest example to implement is an inverter.

Code:

```v
\m5_TLV_version 1d: tl-x.org
\m5
   
\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   $out = !$in;
   
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule
```

Output Diagram and Waveform:

![image](https://github.com/user-attachments/assets/c047d69e-2ac9-4211-8918-6165fa5b17dd)

### Logic Gate (XOR Gate) Implementation

We can also implement the fundamental logic gates. For instance, below is the example for XOR gate: 

Code:

```v
\m5_TLV_version 1d: tl-x.org
\m5
   
\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   $out = $a ^ $b;
   
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule
```

![image](https://github.com/user-attachments/assets/04a6b9e0-f4bc-4893-8818-3061f02d89c1)

Similarly we can code for other operations as follows: 
```v

$out = $a ^ $b; // XOR Gate 
$out = $a & $b; // AND Gate 
$out = $a | $b; // OR Gate 
$out = !($a & $b) ; // NAND Gate 
$out = ($a | $b); // NOR Gate

```
### Multiplexer

Below is the code and output for the multiplexer circuit.

Code:

```v
\m5_TLV_version 1d: tl-x.org
\m5
   
\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   $out = $sel ? $a : $b;
   
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule
```

Output:

![image](https://github.com/user-attachments/assets/72cda6ed-d2ad-4826-bb94-50736234ef78)

Similarly we can pass vectors as the inputs in multiplexer as follows: 

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5
   
\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   $out[7:0] = $sel ? $a[7:0] : $b[7:0];
   
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/babc8740-4e14-45e0-ad6c-209e01ce295c)

### Combinational Basic Calculator

Now, below is the implementation of the basic calulator using 4:1 MUX with operand as the select line and the operation results as the inputs. This will result in the circuit as shown below.

![Comb Ckt](https://github.com/user-attachments/assets/76de903c-6a3f-439f-91d0-e815c28c90ce)

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5

\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   
   $val1[31:0] = $rand1[3:0];
   $val2[31:0] = $rand2[3:0];
   
   $sum[31:0] =  $val1[31:0] +  $val2[31:0];
   $diff[31:0] =  $val1[31:0] -  $val2[31:0];
   $prod[31:0] =  $val1[31:0] *  $val2[31:0];
   $quot[31:0] =  $val1[31:0] /  $val2[31:0];
   
   $out[31:0] = $sel[1] ? ($sel[0] ? $quot[31:0] : $prod[31:0])
                        : ($sel[0] ? $diff[31:0] : $sum[31:0]);
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule
```

Output:

![image](https://github.com/user-attachments/assets/5788b395-c600-4a78-b27c-2534406f892c)

## Sequential Circuits Implementation

### Sequential Calculator

In sequential there is a feedback path from output to input. For instance, in the above calculator example, we can feed the output to value 2 in the circuit. 

![Comb Ckt](https://github.com/user-attachments/assets/c1619151-a30f-4faf-beae-31246dff4fcb)

Below is the code and output waveform:

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5

\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   
   $val1[31:0] = $rand1[3:0];
   $val2[31:0] = >>1$out[31:0]; // Here the output is feeded to val2 in the input after 1 clock cycle i.e., the output is reflected in val2 from the next clock cycle
   $op[1:0] = $rand2[1:0];
   
   $sum[31:0] = $val1[31:0] + $val2[31:0];
   $diff[31:0] = $val1[31:0] - $val2[31:0];
   $prod[31:0] = $val1[31:0] * $val2[31:0];
   $quot[31:0] = $val1[31:0] / $val2[31:0];
   
   $out[31:0] = $reset ? 32'b0 : (($op[1:0]==2'b00) ? $sum :
                                       ($op[1:0]==2'b01) ? $diff :
                                          ($op[1:0]==2'b10) ? $prod : $quot);
   
   `BOGUS_USE($out);
   `BOGUS_USE($reset);
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```

Output: 

![image](https://github.com/user-attachments/assets/c2a8e196-47e7-4e05-9e13-8294cfc255b1)

### Fibonocci Sequence

Now, with the same shifting logic we can implement a fibonocci sequence circuit as below.

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5
   
\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   
   $num[31:0] = $reset ? 1 : (>>1$num + >>2$num);
   
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```

Output: 

![image](https://github.com/user-attachments/assets/302f0ae5-1672-4539-b1df-81f9d9f9f572)

## Pipelining

Pipelining is a method in TL verilog which helps to code the requirments in much compact form factor as compared to that of system verilog. The primary advantage of such is better readability and error solving.

### Fibonocci Sequence

For instance, the fibonocci can be implemented in pipelining as follows:

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5
   
\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   
   |fib
      @1
         $num[31:0] = $reset ? 1 : (>>1$num + >>2$num);
   
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/74ad2d4a-4f53-4e6c-abec-42e8639c786c)

### Pipelining in Basic Calculator

In the next example, the basic calculator as implemented above with pipelining as follows.

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5

\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   
   |calc
      @1
         $val1[31:0] = $rand1[3:0];
         $val2[31:0] = >>1$out;

         $sum[31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $quot[31:0] = $val1 / $val2;
         
         $out[31:0] = $reset ? 0 : ($op[1] ? ($op[0] ? $quot[31:0] : $prod[31:0])
                        : ($op[0] ? $diff[31:0] : $sum[31:0]));

         $cnt[31:0] = $reset ? 0 : >>1$cnt + 1;
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/1a28d748-b1d6-4e6b-9f5c-0d815cf9820a)

Now the same calculator circuit in pipelining can be implemented in two clock cycles whereby in first clock we compute the addition, subtraction, multiplication and division of the two input values and in the second clock we consider the operand to provide the required output.

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5

\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   
   |calc
      @1
         $val1[31:0] = $rand1[3:0];
         $val2[31:0] = >>1$out;

         $sum[31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $quot[31:0] = $val1 / $val2;

         $cnt[31:0] = $reset ? 0 : >>1$cnt + 1;
      
      @2
         $out[31:0] = $reset ? 0 : ($op[1] ? ($op[0] ? $quot[31:0] : $prod[31:0])
                        : ($op[0] ? $diff[31:0] : $sum[31:0]));
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```

Output: 

![image](https://github.com/user-attachments/assets/f0532044-a645-41ff-a19f-e754270cd955)

## Validity

Validity is an additional check applied on top of pipelining in the TL Verilog. 

Also, validity makes it easier to debug the code and handle the error checking. The overall code can be represented in a clenaer way with better readability. 

### Distance Accumulator with Pythagoras Theorem

Below is the example of distance accumumlator with pythagoras theorem.

Circuit Diagram:

![image](https://github.com/user-attachments/assets/521e61ea-a051-4f89-afb1-cede3b0309a9)

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5

\SV
   m5_makerchip_module
\TLV
   |calc
      @1
         $reset = *reset;
         $clk_kar = *clk;
         
         $aa[3:0] ==4'b0011;
         $bb[3:0] ==4'b0100;
         
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa[3:0];
            $bb_sq[31:0] = $bb[3:0] * $bb[3:0];;
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;;
         @3
            $out[31:0] = sqrt($cc_sq);
            
      @4
         $total_distance[63:0] = 
            $reset ? '0 :
            $valid ? >>1$total_distance + $out :
                     >>1$total_distance;
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/05aff26e-1a75-44a8-8ea0-15b313f4308a)

### Lab on 2 Cycle calculator with validity

Now, continuing with the calculator example using validity.

Code:

```v

\m5_TLV_version 1d: tl-x.org
\m5

\SV
   m5_makerchip_module
\TLV
   $reset = *reset;
   $clk_kar = *clk;
   
   |calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $quot[31:0] = $val1 / $val2;
            
         @2   
            $out [31:0] = $reset ? 32'b0 :
                          ($op[1:0] == 2'b00) ? $sum :
                          ($op[1:0] == 2'b01) ? $diff :
                          ($op[1:0] == 2'b10) ? $prod :
                                                $quot ;
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/59d998fc-efd2-4fba-8386-ff96bc33179c)

## Basic RISCV CPU Micro Architecture

The block diagram of the RISCV Micro Architecutre is as shown below.

![image](https://github.com/user-attachments/assets/0241c39a-49fa-4d56-b08e-9f59a837ee64)

### Instruction Fetch

Instruction fetch is the process in a CPU where the next instruction to be executed is retrieved from memory into the instruction register.

Code:

```v

\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/stevehoover/RISC-V_MYTH_Workshop/c1719d5b338896577b79ee76c2f443ca2a76e14f/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 : (>>1$pc + 32'd4);
         
      @1
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $instr[31:0] = $imem_rd_data[31:0]; 
   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)

\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/e5eae0eb-261a-4be1-b0c7-4cbbd95b287e)

### Instruction Decode

Instruction decode is the process in a CPU where the binary instruction fetched from memory is interpreted to determine the operation to be performed and the operands involved.

Code:

```v

\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/stevehoover/RISC-V_MYTH_Workshop/c1719d5b338896577b79ee76c2f443ca2a76e14f/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_kar = *clk;
         $pc[31:0] = >>1$reset ? 32'b0 : (>>1$pc + 32'd4);
         
      @1
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $instr[31:0] = $imem_rd_data[31:0]; 
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001 ||
                       $instr[6:2] ==? 5'b11100;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b0x100 ||
                       $instr[6:2] ==? 5'b01110;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? { {20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? { $instr[31:12], 12'b0} :
                      $is_j_instr ? { {12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} : 32'b0;
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         
         $rs1_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs2_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs2_valid
            $funct3[3:0] = $instr[14:12];
   
         $opcode[6:0] = $instr[6:0];
         
         $funct7_valid = $is_r_instr;
         ?$rs2_valid
            $funct7[6:0] = $instr[31:25];
            
         $dec_bits[10:0] = {$funct[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits == 11'b0_000_0110011;

     
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/dca5dade-84d4-4c6b-83af-331d47e784d3)

![image](https://github.com/user-attachments/assets/6de7e3e0-78be-4b85-aa03-0e82c6185f68)

### Register File Read

Register read is the process in a CPU where data is retrieved from specified registers for use in an instruction's execution.

Code:

```v

\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/stevehoover/RISC-V_MYTH_Workshop/c1719d5b338896577b79ee76c2f443ca2a76e14f/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_kar = *clk;
         $pc[31:0] = >>1$reset ? 32'b0 : (>>1$pc + 32'd4);
         
      @1
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $instr[31:0] = $imem_rd_data[31:0]; 
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001 ||
                       $instr[6:2] ==? 5'b11100;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b0x100 ||
                       $instr[6:2] ==? 5'b01110;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? { {20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? { $instr[31:12], 12'b0} :
                      $is_j_instr ? { {12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} : 32'b0;
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         
         $rs1_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs2_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs2_valid
            $funct3[3:0] = $instr[14:12];
   
         $opcode[6:0] = $instr[6:0];
         
         $funct7_valid = $is_r_instr;
         ?$rs2_valid
            $funct7[6:0] = $instr[31:25];
            
         $dec_bits[10:0] = {$funct[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits == 11'b0_000_0110011;
         
         $rf_rd_en1 = !$rs1_valid;
         $rd_rd_en2 = !$rs2_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_index2[4:0] = $rs2;
         $rf_rd_data1[31:0] = $src1_value[31:0]; 
         $rf_rd_data2[31:0] = $src2_value[31:0];
         
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/049af607-c1d3-40ae-b2a8-d387bb902d30)


![image](https://github.com/user-attachments/assets/0a6b063a-2e10-46f5-a8e0-d126b1ec8100)

### Regsiter File Write

Register write is the process in a CPU where the result of an instruction is stored back into a specified register.

Code: 

```v

\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/stevehoover/RISC-V_MYTH_Workshop/c1719d5b338896577b79ee76c2f443ca2a76e14f/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_kar = *clk;
         $pc[31:0] = >>1$reset ? 32'b0 : (>>1$pc + 32'd4);
         
      @1
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $instr[31:0] = $imem_rd_data[31:0]; 
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001 ||
                       $instr[6:2] ==? 5'b11100;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b0x100 ||
                       $instr[6:2] ==? 5'b01110;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? { {20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? { $instr[31:12], 12'b0} :
                      $is_j_instr ? { {12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} : 32'b0;
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         
         $rs1_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs2_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs2_valid
            $funct3[3:0] = $instr[14:12];
   
         $opcode[6:0] = $instr[6:0];
         
         $funct7_valid = $is_r_instr;
         ?$rs2_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_u_instr || $is_j_instr || $is_i_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7]; 
            
         $dec_bits[10:0] = {$funct[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits == 11'b0_000_0110011;
         
         `BOGUS_USE($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         
         $rf_rd_en1 = $rs1_valid;
         $rd_rd_en2 = $rs2_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_index2[4:0] = $rs2;
         $rf_rd_data1[31:0] = $src1_value[31:0]; 
         $rf_rd_data2[31:0] = $src2_value[31:0];
         
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx;
         
         $rf_wr_en = ($rd_valid || $rd !== 5'b0);  
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
 
\SV
   endmodule

```

Output:

![image](https://github.com/user-attachments/assets/3a05b496-8052-4fa0-99ab-74d4406fa5c4)

![image](https://github.com/user-attachments/assets/618aa0c5-d2b0-4a36-ac48-1d7080e6c810)

## Complete Pipeline RISC-V CPU Micro Architecture

Here’s a breakdown of what the code does, presented as key points:

1. **RISC-V Processor Implementation**: The code implements a basic RISC-V processor in TL-Verilog, handling instruction fetch, decode, and execution.

2. **Instruction Decode and Execution**: It decodes instructions to identify the type (e.g., R-type, I-type) and performs operations like addition or branch based on the instruction.

3. **Register File Operations**: The processor reads from and writes to registers, managing source and destination registers as needed for each instruction.

4. **Pipeline Stages**: Pipeline stages are defined using `@`, with different stages handling different parts of the instruction processing (e.g., fetch, decode, execute).

5. **Simulation Control**: The code includes logic to end the simulation after a certain number of cycles and checks if the simulation passes or fails.

Code:

```v

\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_kar = *clk;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==  11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         //RAW dependence check for ALU data forwarding
         //If previous instruction was writing to reg file, and current instruction is reading from same register
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch resolution
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
      
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //Checks if sum of numbers from 1 to 9 is obtained in reg[17] and runs 10 cycles extra after this is met
   *passed = |cpu/xreg[17]>>10$value == (1+2+3+4+5+6+7+8+9);
   //Run for 200 cycles without any checks
   //*passed = *cyc_cnt > 200;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule

```

Output Waveform:

![image](https://github.com/user-attachments/assets/2fa01f6e-f559-493a-bcc9-730312db1a59)

![image](https://github.com/user-attachments/assets/58bb4a1c-e0d4-45a5-a141-8d5adf778f49)

Output Diagram Snapshots:

![image](https://github.com/user-attachments/assets/cc2c2696-7a8f-496f-aa4f-1332f9496aba)

![image](https://github.com/user-attachments/assets/5eb5b8a7-3672-4071-bbc6-e6a3831d01d1)

![image](https://github.com/user-attachments/assets/5b406c2a-c0a3-45d7-8fc5-611792a802a1)

![image](https://github.com/user-attachments/assets/adeeab42-9492-4c1e-a3d8-d7e47aa6c722)

![image](https://github.com/user-attachments/assets/e06b7dd8-29b4-45fd-8a08-861b0bc22b36)

![image](https://github.com/user-attachments/assets/adde14da-5f08-4259-8d96-fec1724f7df5)

![image](https://github.com/user-attachments/assets/dd26f8b2-32f9-4af4-9d8c-6825eb85fd1a)

![image](https://github.com/user-attachments/assets/21966e82-d088-426f-99d4-13ee4aefe00e)

![image](https://github.com/user-attachments/assets/b782acdb-a89e-4b4f-8bbc-a5974db9e892)

![image](https://github.com/user-attachments/assets/e06340d8-86a6-4d9c-ab52-3662cb987197)

Output Viz:

![image](https://github.com/user-attachments/assets/7ece731d-c595-4342-80b4-d25f1a111386)

![image](https://github.com/user-attachments/assets/3dc85106-b255-478c-b157-dc08e22261dd)


</details>
