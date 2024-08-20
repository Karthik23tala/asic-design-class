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

## Combinational Circuits Implementation

Makerchip is an online IDE in which we can write the code in TL Verilog and simulate to observe the diagram, waveforms and also the visualization.

Below is the snapshot of the IDE.

![Makerchip IDE](https://github.com/user-attachments/assets/44a636e1-33ca-4b92-adb5-3c984f8f0ad7)

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

![image](https://github.com/user-attachments/assets/84193687-0a2d-4c22-a5ae-a749764f7fab)

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

![image](https://github.com/user-attachments/assets/63fb7773-81c7-4889-a347-f6dc2dd82dd1)

Similarly we can code for other operations as follows: 
```v

$out = $a ^ $b; // XOR Gate 
$out = $a & $b; // AND Gate 
$out = $a | $b; // OR Gate 
$out = !($a & $b) ; // NAND Gate 
$out = ($a | $b); // NOR Gate

```

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

![image](https://github.com/user-attachments/assets/f6622d2d-f2ab-4a95-b4a8-d1ec76b6e846)

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

![image](https://github.com/user-attachments/assets/7bda4c15-1647-4506-8e2e-7c97d3354d8e)

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

![image](https://github.com/user-attachments/assets/f4c66660-5e28-4498-8cb9-9520a481122e)

## Sequential Circuits Implementation

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

![image](https://github.com/user-attachments/assets/b0c09e4b-2fd0-4a90-8e08-a2b08358146d)

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

![image](https://github.com/user-attachments/assets/ef730a0b-3be8-41b9-b2e5-39af9695ce8b)

## Pipelining

Pipelining is a method in TL verilog which helps to code the requirments in much compact form factor as compared to that of system verilog. The primary advantage of such is better readability and error solving.

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

![image](https://github.com/user-attachments/assets/aaec92b4-aa76-45c8-bdc8-5241c42cb6fd)

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

![image](https://github.com/user-attachments/assets/4474f58c-244d-4982-b73b-853915d172be)

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



</details>
