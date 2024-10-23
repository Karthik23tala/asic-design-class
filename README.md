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

Generated Waveform Pre Synthesis simulation:

Zoomed out results showing multiple cycles:

![image](https://github.com/user-attachments/assets/f3e55a35-42eb-45c0-bb36-050f9cc097f3)

Zoomed in results showing the sum of numbers 1 to 9:

![image](https://github.com/user-attachments/assets/f060765f-62cd-4a44-aec4-4e9918c25993)

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

## Comparison of Pre vs Post Synthesis Simulation Waveform:

### Waveform Screenshot Pre-Synthesis:

![image](https://github.com/user-attachments/assets/f060765f-62cd-4a44-aec4-4e9918c25993)

### Waveform Screenshot Post-Synthesis: 

![image](https://github.com/user-attachments/assets/4c3b64a0-df18-4fb7-8ce0-ef4a644cddf4)

### Conclusion: From the above comparison of waveforms from both the synthesis we can conclude that the acheived output matches the expectation i.e., the sum of numbers from 1 to 9 which is 45 in decimal or 2D in hexadecimal format.
</details>
