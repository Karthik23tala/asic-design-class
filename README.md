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

### Problem Statement:
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
         $clk_kar = *clk;
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

![image](https://github.com/user-attachments/assets/4e556a5d-5244-493b-8d41-a0d2a6766d2c)

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

![image](https://github.com/user-attachments/assets/439ec3f0-513a-4be6-ab06-8b975def7da2)

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

<details>
<summary>Lab 6</summary>
<br>

# ASIC Lab 6 Report (27/08/2024)

In this lab, we have simulated the same sum 1 to 9 program as written in previous lab and compared the output waveforms of gtkwave and makerchip platform.

## Simulation of Sum 1 to 9 program in Sandpiper and GTKWave

Below are the steps:

**Step 1** : Install the required packages

To install the python3, Sandpiper and gtkwave packages with the below commands

```

sudo apt install make python python3 python3-pip git iverilog gtkwave
sudo apt-get install python3-venv
pip3 install pyyaml click sandpiper-saas
python3 -m venv .venv
source ~/.venv/bin/activate
sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
sudo chmod 666 /var/run/docker.sock

```

**Step 2** : Clone the git

Next, clone the git with the below command.

```

git clone https://github.com/manili/VSDBabySoC.git

```

**Step 3** : Replacing the ```.tlv``` file

In the cloned git, navigate to VSDBabySoC/src/module and replace the code present in rvmyth.tlv to the code for sum 1 to 9.

**Step 4** : Navigate to cd VSDBabySoC and convert the ```.tlv``` file to ```.v``` 

The code is written in transaction level verilog language. To simulate it needs to be converted to verilog format with the following command.

```

sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/

```

**Step 5** : Create the ```.vcd``` file to view the waveform

Post conversion of the verilog code from ```.tlv``` file to ```.v``` , ```.vcd``` file needs to be created with the following command.

```

make pre_synth_sim

```

**Step 6** : Compile and simulate the verilog code

Simulate the verilog code in iverilog simulator

```

iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module

```

**Step 7** : View the output waveform

Finally, to view the output waveform in the earlier created ```.vcd``` file run the below commands.

```

cd output
./pre_synth_sim.out

```

GTKWave Output Waveform:

![image](https://github.com/user-attachments/assets/c6807f19-5085-489b-a94f-dcada9b7868e)

Below is the makerchip output waveform for comparison.

Makerchip Output Waveform:

![image](https://github.com/user-attachments/assets/39bc031b-920c-4e40-a795-0536dc311974)

**Conclusion** : By comparing the output waveforms of both gtkwave and makerchip we can conclude that the acheived output matches the expectation i.e., the sum of numbers from 1 to 9 which is 45 in decimal or 2D in hexadecimal format.

</details>


<details>
<summary>Lab 7</summary>
<br>

# ASIC Lab 7 Report (29/08/2024)

In this lab, we have used the peripherals namely PLL (Phase Locked Loop) and DAC (Digital to Analog Converter) to convert the digital output obtained in the previous lab from digital format to analog format.

## Definitions

- **Phase Locked Loop:** A **Phase-Locked Loop (PLL)** is an electronic circuit that synchronizes the phase of an output signal with a reference signal by continuously adjusting its frequency. It is widely used in applications like frequency synthesis, clock recovery, and demodulation to maintain signal stability and synchronization.
  
- **Digital to Analog Converter:** A **Digital-to-Analog Converter (DAC)** is an electronic device that converts digital signals, typically binary, into analog voltages or currents. It is commonly used in applications like audio playback, where digital audio data is converted to analog signals for speakers or other output devices.

Now, we need to run the ```rvmyth.v``` file, copy the output to ```.vcd``` file and observe the obtained waveform in GTKWave.

Below is the list of commands to use.

```
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

Terminal Screenshots:

![image](https://github.com/user-attachments/assets/96f0bb4f-3dd5-4d6a-bd5d-2cd20495f753)

Output Waveform Screenshot:

![image](https://github.com/user-attachments/assets/9506b968-2500-485d-a5b4-5e2136721ca5)

</details>

<details>
<summary>Lab 8</summary>
<br>

# ASIC Lab 8 Report (15/10/2024) - Sky130 RTL Design Workshop
<br>

<details>
<summary>Day 1</summary>
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

</details>

<details>
<summary>Day 2</summary>
<br>

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

</details>

<details>
<summary>Day 3</summary>
<br>

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

</details>

<details>
<summary>Day 4</summary>
<br>

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

### ```blocking_caveat.v```

Code:

```v
module blocking_caveat(input a, input b, input c, output reg d);
	reg x;

	always@(*)
	begin
		d = x & c;
		x = a | b;
	end
endmodule
```

Commands and Screenshots:

```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr blocking_caveat_net.v
!gvim blocking_caveat_net.v
```

![image](https://github.com/user-attachments/assets/a5ba7037-65d7-4a03-a534-c8c671e973ba)

From the above waveform, we can observe an instance where a = 1, b = 0 and c = 1 but the output d = 0 which is incorrect. 

This incorrect output is due to the usage of blocking statement whereby the circuit has inferred the previous values of a and b for the output calculation.

![image](https://github.com/user-attachments/assets/6d65fee2-b825-4641-aa75-f887ac119e9c)

![image](https://github.com/user-attachments/assets/cc4e7a9c-890d-4ebc-895f-abf35d26e650)

![image](https://github.com/user-attachments/assets/cb24aa30-7d6c-4eff-8aeb-3f22ce72450f)

Gate-level synthesis commands and screenshots:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![image](https://github.com/user-attachments/assets/032c7954-b8c2-4b99-a77b-a02c4b8aa285)

![image](https://github.com/user-attachments/assets/39d5b6d2-b990-4a34-9a37-811eaba24720)

In the above waveform generated through gate level synthesis we can observe that the occurence mismatch is removed and we obtain the proper output as per the instantaneous values of the input signals.

# Conclusion: In summary we can conclude that the mismatch error is resolved and there is no blocking occurence post the simulation through gate level synthesis (GLS)

</details>

</details>

<details>
<summary>Lab 9</summary>
<br>

# Lab 9 Report (22/10/24) - RVMYTH verilog file synthesis using Yosys and Post Sythesis simulation using iverilog and GTKWave
<br>

## Pre Synthesis Simulation

Commands:

```
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

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

## Comparison of Pre vs Post Synthesis Simulation Waveform:

### Waveform Screenshot Pre-Synthesis:

![image](https://github.com/user-attachments/assets/f060765f-62cd-4a44-aec4-4e9918c25993)

![image](https://github.com/user-attachments/assets/f3e55a35-42eb-45c0-bb36-050f9cc097f3)

### Waveform Screenshot Post-Synthesis: 

![image](https://github.com/user-attachments/assets/4c3b64a0-df18-4fb7-8ce0-ef4a644cddf4)

![image](https://github.com/user-attachments/assets/9b50a9d0-c918-48ef-88cc-83b2a46a16cb)

### Conclusion: From the above comparison of waveforms from both the synthesis we can conclude that the acheived output matches the expectation i.e., the sum of numbers from 1 to 9 which is 45 in decimal or 2D in hexadecimal format.
</details>

<details>
<summary>Lab 10</summary>
<br>

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

</details>

<details>
<summary>Lab 11</summary>
<br>

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

</details>

<details>

<summary> Lab 12 </summary>

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

![image](https://github.com/user-attachments/assets/bc114bb2-83ba-40cb-a9bb-76690de57d95)


7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.
Noting down current design values generated before modifying parameters to improve timing

![image](https://github.com/user-attachments/assets/98905dab-4d9d-4623-8cf2-e2668132cd99)

![image](https://github.com/user-attachments/assets/a2ed0ed5-fe97-4e07-b15a-76edffa01a75)

<img width="152" alt="image" src="https://github.com/user-attachments/assets/25b4ab8b-5ac7-4e50-8839-25829e60a98c">


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

<img width="757" alt="image" src="https://github.com/user-attachments/assets/6f1918b1-95e8-42c3-9f50-eac41bc4a009">

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
prep -design picorv32a -tag 13-11_15-13 -overwrite

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

![image](https://github.com/user-attachments/assets/f89f8cdf-0013-4847-82b5-207863a3e3ff)

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

<img width="380" alt="image" src="https://github.com/user-attachments/assets/eeb8a493-d08e-4a53-a30c-bc848be25487">

OR gate of drive strength 2 is driving 4 fanouts

<img width="371" alt="image" src="https://github.com/user-attachments/assets/56ebcba6-6a42-4cb5-ab85-965133055454">

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

OR gate of drive strength 2 driving OA gate has more delay

<img width="392" alt="image" src="https://github.com/user-attachments/assets/713c6771-9058-4f9d-8f18-75efed91166f">

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```
# Reports all the connections to a net
report_net -connections _11643_

# Replacing cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Commands to verify instance _14506_ is replaced with sky130_fd_sc_hd__or4_4
```
# Generating custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_
```

<img width="261" alt="image" src="https://github.com/user-attachments/assets/0a8c352d-8abc-4116-b4ea-7d9d5356c24a">

Hence the total TNS and WNS after all the optimisations of the gates is as follows:

**TNS:  -759.46 to -711.59
WNS: -24.89 to -23.89 **


11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Now to insert this updated netlist to PnR flow and we can use write_verilog and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist

Commands to make copy of netlist
```
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_15-13/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```

![image](https://github.com/user-attachments/assets/171028f8-897a-4506-a3ba-912d9826892a)


Commands to write verilog
```
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_15-13/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```

![image](https://github.com/user-attachments/assets/2c857a50-3eeb-4aa1-8b34-af4ba21fdec3)


Verified that the netlist is overwritten by checking that instance _14506_ is replaced with sky130_fd_sc_hd__or4_4

Since we confirmed that netlist is replaced and will be loaded in PnR but since we want to follow up on the earlier 0 violation design we are continuing with the clean design to further stages

Commands load the design and run necessary stages
```
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 13-11_15-13 -overwrite

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

![image](https://github.com/user-attachments/assets/8c9007a1-36c0-4eba-85cc-375e820c235f)

![image](https://github.com/user-attachments/assets/aff36464-bf19-40e2-9d1c-ec5d181a537a)

![image](https://github.com/user-attachments/assets/a4d2d316-a038-48d6-9991-13e3a02e5874)

12. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD
```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_15-13/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_15-13/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_15-13/results/synthesis/picorv32a.synthesis_cts.v

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

<img width="410" alt="image" src="https://github.com/user-attachments/assets/988ba67a-53cd-4603-b5ed-334f924777ac">

<img width="386" alt="image" src="https://github.com/user-attachments/assets/6de3dd0a-ecf2-49e4-a7e1-8656e235a878">


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
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/13-11_15-13/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_15-13/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_15-13/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_15-13/results/synthesis/picorv32a.synthesis_cts.v

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

<img width="411" alt="image" src="https://github.com/user-attachments/assets/bc4a6aa5-72d6-484c-b107-1f2e8ae03df6">

<img width="400" alt="image" src="https://github.com/user-attachments/assets/cf79d044-a354-455d-8cb9-266c3fe5552e">


## Day 5:  Final steps for RTL2GDS using tritonRoute and openSTA 

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

![image](https://github.com/user-attachments/assets/1902f5c8-8826-4a14-bdc6-a3ab0905b039)

![image](https://github.com/user-attachments/assets/04ac66d5-6423-48fb-83a2-197d418d2810)

![image](https://github.com/user-attachments/assets/22adbbdf-9d15-4794-8f17-244d89522f70)

![image](https://github.com/user-attachments/assets/6ad4b9e3-05b5-41f8-91af-54aae1cc1342)

![image](https://github.com/user-attachments/assets/e7e5b985-7eec-4194-9067-fdccd7315028)

![image](https://github.com/user-attachments/assets/78401964-26a0-4774-a698-1d32fbe92734)

Commands to load PDN def in magic in another terminal
```
# Change directory to path containing generated PDN def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_21-01/tmp/floorplan/

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
Screenshots of PDN def

![image](https://github.com/user-attachments/assets/6ee7fabe-f206-4d5e-b089-58ed495203cf)

![image](https://github.com/user-attachments/assets/53619bfa-0e4f-4a24-bc23-8b00429260e9)

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

![Screenshot 2024-11-15 033612](https://github.com/user-attachments/assets/e14708fe-87e3-4922-89e1-1faf2d0db6db)

![Screenshot 2024-11-15 033636](https://github.com/user-attachments/assets/78ca8c79-a999-4aea-a96f-6c42ab4cb70c)

Commands to load routed def in magic in another terminal
```
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_15-13/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```
Screenshots of routed def

![Screenshot 2024-11-15 035252](https://github.com/user-attachments/assets/0eae5500-cac3-4980-92a5-1926893c8ee7)

![Screenshot 2024-11-15 035325](https://github.com/user-attachments/assets/16a2363e-38ac-4b7f-a952-9891b14f7689)

![Screenshot 2024-11-15 035428](https://github.com/user-attachments/assets/d96f0476-72fe-444f-830f-f977d46c06be)

![Screenshot 2024-11-15 035058](https://github.com/user-attachments/assets/71d4f84b-d3a8-4f06-8e96-b2e741474e7f)

Screenshot of fast route guide present in openlane/designs/picorv32a/runs/13-11_21-01/tmp/routing directory

![Screenshot 2024-11-15 035632](https://github.com/user-attachments/assets/97e0c46a-60d5-4ca2-a9f1-f3b9e6eb362c)

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

![Screenshot 2024-11-15 040222](https://github.com/user-attachments/assets/fcb8299c-5454-4f52-b1e0-0e6731eaa3de)

![Screenshot 2024-11-15 041629](https://github.com/user-attachments/assets/a48c5df9-f9da-49e4-bb67-af3329059353)

<img width="456" alt="image" src="https://github.com/user-attachments/assets/4c6b0f1d-5f28-4812-a51e-6d7315f908cc">

<img width="424" alt="image" src="https://github.com/user-attachments/assets/2eb372d5-f040-4291-96c9-5484f423bb52">

![image](https://github.com/user-attachments/assets/a50a7a15-ba11-4cfc-8006-762f5b4de552)


</details>

<details>
<summary> Lab 13 </summary>

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

![image](https://github.com/user-attachments/assets/ba58037d-6148-49f4-bcd4-9bf6a22200ab)

![image](https://github.com/user-attachments/assets/7ddf822a-2ed4-4da4-9a96-631a35c84320)

Makerfile Screenshots:

![image](https://github.com/user-attachments/assets/2f05afd2-1e52-456d-8012-68af2a91ee4a)

![image](https://github.com/user-attachments/assets/eb6df4ad-5726-4f65-a2de-10f81308a34a)

![image](https://github.com/user-attachments/assets/e4a11418-43a2-481b-94ab-7d2047ca256a)

![image](https://github.com/user-attachments/assets/8cf3486a-a73b-4ce2-87ba-a1028d150838)

![image](https://github.com/user-attachments/assets/84469488-e4e5-4856-ab0a-52d7a157b7aa)

![image](https://github.com/user-attachments/assets/fb2104b9-6f69-4906-8acb-297e060f226a)

![image](https://github.com/user-attachments/assets/e16635c0-56ff-4656-baa0-b513d7f54b33)

![image](https://github.com/user-attachments/assets/daaa0619-485f-45aa-b795-ea9a7a4d01fd)

**Automated RTL-to-GDS Flow for VSDBabySoC**

Initial Setup:
Create Required Directories

Inside OpenROAD-flow-scripts/flow/designs/sky130hd, create a new directory named vsdbabysoc.
Similarly, create another directory named vsdbabysoc within OpenROAD-flow-scripts/flow/designs/src. Place all Verilog files for VSDBabySoC in this directory.
Copy Supporting Files

Copy the folders gds, include, lef, and lib from the VSDBabySoC directory on your system into the newly created vsdbabysoc directory.
The contents of these folders should include the following:
gds Folder: Contains avsddac.gds and avsdpll.gds.
include Folder: Contains the files sandpiper.vh, sandpiper_gen.vh, sp_default.vh, and sp_verilog.vh.
lef Folder: Contains avsddac.lef and avsdpll.lef.
lib Folder: Contains avsddac.lib and avsdpll.lib.
Add Constraints File

Copy the constraints file (vsdbabysoc_synthesis.sdc) from the VSDBabySoC directory on your system into the vsdbabysoc directory.
Include Configuration Files

Copy the configuration files macro.cfg and pin_order.cfg from the VSDBabySoC directory on your system into the same directory.

```
export DESIGN_NICKNAME = vsdbabysoc
export DESIGN_NAME = vsdbabysoc
export PLATFORM    = sky130hd

# export VERILOG_FILES_BLACKBOX = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/IPs/*.v
# export VERILOG_FILES = $(sort $(wildcard $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/*.v))
# Explicitly list the Verilog files for synthesis
export VERILOG_FILES = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/vsdbabysoc.v \
                       /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/module/rvmyth.v \
                       /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/clk_gate.v

export SDC_FILE      = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/sdc/vsdbabysoc_synthesis.sdc

#export DIE_AREA   = 0 0 1500 1500
# export CORE_AREA  = 10 10 2910 3510

# export PLACE_DENSITY ?= 0.23

export vsdbabysoc_DIR = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc

export VERILOG_INCLUDE_DIRS = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/include

# export SDC_FILE      = $(wildcard $(vsdbabysoc_DIR)/sdc/*.sdc)
export ADDITIONAL_GDS  =/home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/gds
export ADDITIONAL_LEFS  = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/lef

export ADDITIONAL_LIBS = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/lib/avsddac.lib \
				         /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/src/lib/avsdpll.lib
# Clock Configuration (vsdbabysoc specific)
# export CLOCK_PERIOD = 20.0
export CLOCK_PORT = CLK
export CLOCK_NET = $(CLOCK_PORT)

# Floorplanning Configuration (vsdbabysoc specific)
export FP_PIN_ORDER_CFG = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/vsdbabysoc/pin_order.cfg
export FP_SIZING = absolute
export DIE_AREA = 0 0 2000 2000
export CORE_AREA = 10 10 1800 1800


# export PL_RESIZER_HOLD_MAX_BUFFER_PERCENT = 80
# export PL_RESIZER_HOLD_MAX_BUFFER_COUNT = 5000  # Set the buffer limit to a higher value if needed
# export GLB_RESIZER_HOLD_MAX_BUFFER_PERCENT = 80

# Hold Slack Margin Configuration
# export PL_RESIZER_HOLD_SLACK_MARGIN = 0.01
# export GLB_RESIZER_HOLD_SLACK_MARGIN = 0.01


export BOTTOM_MARGIN_MULT = 50
export TOP_MARGIN_MULT = 50
export LEFT_MARGIN_MULT = 200
export RIGHT_MARGIN_MULT = 200

# Placement Configuration (vsdbabysoc specific)
export MACRO_PLACEMENT_CFG = /home/karthik-talakokkula/OpenRoad/flow/designs/sky130hd/VSDBabySoC/macro.cfg

# Magic Tool Configuration
export MAGIC_ZEROIZE_ORIGIN = 0
export MAGIC_EXT_USE_GDS = 1

export SYNTH_HIERARCHICAL = 1

# export RTLMP_BOUNDARY_WT = 0
#  MACRO_PLACE_HALO = 100 100
# export MACRO_PLACE_CHANNEL = 200 200

# CTS tuning
# export CTS_BUF_DISTANCE = 600
# export SKIP_GATE_CLONING = 1

# export SETUP_SLACK_MARGIN = 0.2

# This is high, some SRAMs should probably be converted
# to real SRAMs and not instantiated as flops
# export SYNTH_MEMORY_MAX_BITS ?= 42000

```

![image](https://github.com/user-attachments/assets/3850fbc8-a1d9-41e5-8eef-a9c63890dbc0)

![image](https://github.com/user-attachments/assets/6148f560-c331-429a-a098-f81e7bbe16a8)

Terminal Command and Screenshots for Synthesis:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

![image](https://github.com/user-attachments/assets/3009fc6a-2d35-44eb-bcf7-a6763c1f2bab)

Synthesis Netlist:

![image](https://github.com/user-attachments/assets/e0e2fd1f-7788-4bc0-8f17-6ff4ab23b357)

Synthesis Log:

![image](https://github.com/user-attachments/assets/7f6f1da3-96d3-4a8a-a3e2-1110f891addd)

Synthesis Check:

![image](https://github.com/user-attachments/assets/674065b7-bd3b-46c3-b1d8-07e77700d1ad)

Synthesis Stats:

![image](https://github.com/user-attachments/assets/6ec050f7-47d0-44d0-ae44-4fc90b5b0dfb)

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

![image](https://github.com/user-attachments/assets/af89f6fb-f34b-4204-a68a-8ea0e933a3c6)

![image](https://github.com/user-attachments/assets/efcb189c-bd62-4bc9-bb00-e26ef92fb562)

![image](https://github.com/user-attachments/assets/432df406-a0f9-4982-a776-9f572f5d3eef)

Command to view the floorplanning:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

![image](https://github.com/user-attachments/assets/7563bdab-1743-45f0-8d61-5b977735e1aa)

![image](https://github.com/user-attachments/assets/48ea0abf-1ff7-4879-9192-8d3fda5388b5)

![image](https://github.com/user-attachments/assets/32da2071-9b15-47c7-931b-b95b7da4517a)

Floorplan Log File:

![image](https://github.com/user-attachments/assets/9ca3d628-641a-453c-9541-2c1333ef3d14)

Floorplan Timing Report:

![image](https://github.com/user-attachments/assets/553f4d1a-c64e-49a2-b7ef-ba5616ce7f00)

```

==========================================================================
floorplan final report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
floorplan final report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
floorplan final report_worst_slack
--------------------------------------------------------------------------
worst slack INF

==========================================================================
floorplan final report_checks -path_delay min
--------------------------------------------------------------------------
No paths found.

==========================================================================
floorplan final report_checks -path_delay max
--------------------------------------------------------------------------
No paths found.

==========================================================================
floorplan final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_reset_a3$_DFF_P_ (rising edge-triggered flip-flop)
Endpoint: core.CPU_Xreg_value_a4[10][13]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop)
Path Group: unconstrained
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.00    0.00    0.00 ^ core.CPU_reset_a3$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   591    1.46   13.42    9.67    9.67 ^ core.CPU_reset_a3$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_reset_a3 (net)
                 13.42    0.00    9.67 ^ _10124_/A (sky130_fd_sc_hd__inv_1)
   464    1.04    0.00   21.17   30.85 v _10124_/Y (sky130_fd_sc_hd__inv_1)
                                         _04513_ (net)
                  0.00    0.00   30.85 v _10227_/A (sky130_fd_sc_hd__nand3_1)
    31    0.08    0.78    0.52   31.37 ^ _10227_/Y (sky130_fd_sc_hd__nand3_1)
                                         _04613_ (net)
                  0.78    0.00   31.37 ^ _10232_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.20    0.22   31.59 v _10232_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _00548_ (net)
                  0.20    0.00   31.59 v core.CPU_Xreg_value_a4[10][13]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                 31.59   data arrival time
-----------------------------------------------------------------------------
(Path is unconstrained)



==========================================================================
floorplan final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             7.92e-12   3.67e-12   1.45e-08   1.45e-08  58.0%
Combinational          1.01e-11   1.77e-11   1.04e-08   1.05e-08  42.0%
Clock                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  1.80e-11   2.13e-11   2.49e-08   2.49e-08 100.0%
                           0.1%       0.1%      99.8%
```

Command for Placement:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

![image](https://github.com/user-attachments/assets/b9c1a6a6-54ae-4d62-b6d0-de6ee80fd1bc)

![image](https://github.com/user-attachments/assets/6c42414e-75d2-40e7-9292-ece8583812f1)

![image](https://github.com/user-attachments/assets/4a0e619c-fed5-44c8-8730-18b35d07246a)


```
make gui_place
```

![image](https://github.com/user-attachments/assets/01796517-c2da-4138-8587-a014958b2cc4)

![image](https://github.com/user-attachments/assets/3d95620b-85a0-4e71-a718-361ee94f4bd0)

Timing Report:

![image](https://github.com/user-attachments/assets/22a2ddae-7527-4449-9ff6-d5efd0d18003)

```

==========================================================================
detailed place report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
detailed place report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
detailed place report_worst_slack
--------------------------------------------------------------------------
worst slack INF

==========================================================================
detailed place report_checks -path_delay min
--------------------------------------------------------------------------
No paths found.

==========================================================================
detailed place report_checks -path_delay max
--------------------------------------------------------------------------
No paths found.

==========================================================================
detailed place report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_taken_br_a4$_DFF_P_ (rising edge-triggered flip-flop)
Endpoint: core.CPU_Xreg_value_a4[15][18]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop)
Path Group: unconstrained
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.76    0.00    0.00 ^ core.CPU_valid_taken_br_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.04    0.46    0.46 v core.CPU_valid_taken_br_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_valid_taken_br_a4 (net)
                  0.04    0.00    0.46 v _07913_/B (sky130_fd_sc_hd__or4_4)
    41    0.23    0.36    0.84    1.30 v _07913_/X (sky130_fd_sc_hd__or4_4)
                                         _02930_ (net)
                  0.36    0.02    1.31 v load_slew103/A (sky130_fd_sc_hd__buf_16)
    36    0.28    0.15    0.35    1.66 v load_slew103/X (sky130_fd_sc_hd__buf_16)
                                         net103 (net)
                  0.15    0.01    1.68 v max_cap102/A (sky130_fd_sc_hd__buf_16)
    24    0.27    0.14    0.26    1.94 v max_cap102/X (sky130_fd_sc_hd__buf_16)
                                         net102 (net)
                  0.16    0.04    1.97 v _07915_/A (sky130_fd_sc_hd__clkinv_16)
    43    0.48    0.31    0.25    2.22 ^ _07915_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _02932_ (net)
                  0.37    0.11    2.33 ^ _09981_/C (sky130_fd_sc_hd__nor3_2)
     2    0.02    0.10    0.13    2.46 v _09981_/Y (sky130_fd_sc_hd__nor3_2)
                                         _04371_ (net)
                  0.10    0.00    2.46 v _09982_/B1 (sky130_fd_sc_hd__a21oi_4)
     6    0.10    0.69    0.57    3.02 ^ _09982_/Y (sky130_fd_sc_hd__a21oi_4)
                                         _04372_ (net)
                  0.69    0.00    3.02 ^ _09988_/A2 (sky130_fd_sc_hd__o21ai_4)
    16    0.12    0.32    0.36    3.39 v _09988_/Y (sky130_fd_sc_hd__o21ai_4)
                                         _04378_ (net)
                  0.32    0.00    3.39 v _11217_/A (sky130_fd_sc_hd__nor3_4)
    14    0.11    1.07    0.96    4.35 ^ _11217_/Y (sky130_fd_sc_hd__nor3_4)
                                         _05443_ (net)
                  1.07    0.00    4.35 ^ wire20/A (sky130_fd_sc_hd__buf_8)
    10    0.11    0.19    0.30    4.65 ^ wire20/X (sky130_fd_sc_hd__buf_8)
                                         net20 (net)
                  0.20    0.01    4.66 ^ _11238_/B (sky130_fd_sc_hd__nand2_4)
     5    0.04    0.22    0.13    4.79 v _11238_/Y (sky130_fd_sc_hd__nand2_4)
                                         _05460_ (net)
                  0.22    0.00    4.79 v _11253_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.23    0.23    5.02 ^ _11253_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _00713_ (net)
                  0.23    0.00    5.02 ^ core.CPU_Xreg_value_a4[15][18]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.02   data arrival time
-----------------------------------------------------------------------------
(Path is unconstrained)



==========================================================================
detailed place report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
detailed place max_slew_check_slack
--------------------------------------------------------------------------
0.06809719651937485

==========================================================================
detailed place max_slew_check_limit
--------------------------------------------------------------------------
1.4951549768447876

==========================================================================
detailed place max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0455

==========================================================================
detailed place max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_capacitance_check_slack
--------------------------------------------------------------------------
0.007044170051813126

==========================================================================
detailed place max_capacitance_check_limit
--------------------------------------------------------------------------
0.19410200417041779

==========================================================================
detailed place max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.0363

==========================================================================
detailed place max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
detailed place max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
detailed place max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
detailed place setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
detailed place hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
detailed place report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
No paths found.

==========================================================================
detailed place report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
No paths found.

==========================================================================
detailed place critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path delay
--------------------------------------------------------------------------
-1

==========================================================================
detailed place critical path slack
--------------------------------------------------------------------------
0

==========================================================================
detailed place slack div critical path delay
--------------------------------------------------------------------------
0.000000

==========================================================================
detailed place report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             9.16e-12   1.10e-11   1.45e-08   1.46e-08  49.9%
Combinational          1.89e-11   4.94e-11   1.46e-08   1.46e-08  50.1%
Clock                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  2.80e-11   6.04e-11   2.91e-08   2.92e-08 100.0%
                           0.1%       0.2%      99.7%
```
Command for CTS:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

![image](https://github.com/user-attachments/assets/8728531e-52f8-4d7c-b5bc-6f62b85dbfba)

```
make gui_cts
```

![image](https://github.com/user-attachments/assets/7d4c2181-9c74-4278-8c7c-1a79cb50d1d4)

![image](https://github.com/user-attachments/assets/0e5231f9-54ba-47d4-b43f-0cb59e560a6b)

![image](https://github.com/user-attachments/assets/0df60fd1-343b-41be-b29d-710681af3b22)

Clock Tree Synthesis Screenshots:

![image](https://github.com/user-attachments/assets/8350e52b-d971-4691-af55-b70207f42172)

![image](https://github.com/user-attachments/assets/f4de6fca-7d29-4ac8-8148-3a6d80a3c563)

![image](https://github.com/user-attachments/assets/ccdc7a5c-33a2-49b1-a8fa-de67c3fbdbb5)

Final Layout Commands:

```
make gui_final
```

![image](https://github.com/user-attachments/assets/17601d82-6a8f-4219-9643-a4304930a00c)

![image](https://github.com/user-attachments/assets/bd03ec92-a5a7-4ce2-b6fe-74e8e1bc00e4)

![image](https://github.com/user-attachments/assets/e0f1cad9-0cd9-4281-b995-a02c26677529)

![image](https://github.com/user-attachments/assets/bac41511-624d-42d4-92ae-1c1c920fe061)

![image](https://github.com/user-attachments/assets/0c71875b-98c3-4d07-a754-6909defa67c7)

![image](https://github.com/user-attachments/assets/056ebef4-939b-4010-8307-1f4e4de89531)

Command for Route:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```

![image](https://github.com/user-attachments/assets/dc7a49cd-ea40-4a24-815f-28c05362440a)

![image](https://github.com/user-attachments/assets/090bd5cb-8e66-4b8c-8ab1-18275ab1eac2)

GUI for Route:

```
make gui_route
```

![image](https://github.com/user-attachments/assets/36757b0e-f655-4163-92ad-4701dba80725)

![image](https://github.com/user-attachments/assets/a554b4cb-4d24-4363-bcba-eec33d7c08a5)

**HeatMap Images**

1. VSDBabySoc

![1](https://github.com/user-attachments/assets/138289f3-9b34-4545-8e07-1aa8d8d1d216)

2. Power

![2](https://github.com/user-attachments/assets/8e19b1e8-af50-40be-9b6d-8cc263ec9a72)

3. Estimated Congestion

![3](https://github.com/user-attachments/assets/6634fc26-d406-4be0-9329-bc2e5c69ea53)

4. Routing Congestion

![4](https://github.com/user-attachments/assets/ba076b73-2549-495e-aef5-5d1ced8a5b20)

5. Placement

![5](https://github.com/user-attachments/assets/8c289429-343a-4947-93ef-b7375b734377)

6. IR Drop

![6](https://github.com/user-attachments/assets/35de6190-d8fe-4617-b288-bb9b9c5ecc5a)

**QoR Report**

![image](https://github.com/user-attachments/assets/597095c9-f845-45ef-b172-89033df10ec2)

![image](https://github.com/user-attachments/assets/c4a781ab-fd36-465b-8f35-dcc76913e287)

</details>
