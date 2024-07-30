# Lab 3

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

| Instruction  | Type | Opcode | rs2  | rs1  | funct7 | funct3 | rd   | imm                | 32-bit Instruction Code |
|--------------|------|--------|------|------|--------|--------|------|--------------------|--------------------------|
| ADD r1, r2, r3  | R    | 0110011 | 00011 | 00010 | 0000000 | 000   | 00001 | -                  | 0x000282B3 |
| SUB r3, r1, r2  | R    | 0110011 | 00010 | 00001 | 0100000 | 000   | 00011 | -                  | 0x400282B3 |
| AND r2, r1, r3  | R    | 0110011 | 00011 | 00001 | 0000000 | 111   | 00010 | -                  | 0x00C30333 |
| OR r8, r2, r5   | R    | 0110011 | 00101 | 00010 | 0000000 | 110   | 01000 | -                  | 0x0002B233 |
| XOR r8, r1, r4  | R    | 0110011 | 00100 | 00001 | 0000000 | 100   | 01000 | -                  | 0x0001B233 |
| SLT r10, r2, r4 | R    | 0110011 | 00100 | 00010 | 0000000 | 010   | 01010 | -                  | 0x0002A233 |
| ADDI r12, r3, 5 | I    | 0010011 | -    | 00011 | -      | 000   | 01100 | 000000000101       | 0x00530313 |
| SW r3, r1, 4    | S    | 0100011 | 00011 | 00001 | -      | 010   | -    | 000000000100       | 0x00412123 |
| SRL r16, r11, r2| R    | 0110011 | 00010 | 01011 | 0000000 | 101   | 10000 | -                  | 0x000B9313 |
| BNE r0, r1, 20  | B    | 1100011 | -    | 00001 | -      | 001   | -    | 000000000101       | 0x00514163 |
| BEQ r0, r0, 15  | B    | 1100011 | -    | 00000 | -      | 000   | -    | 000000000111       | 0x00700063 |
| LW r13, r11, 2  | I    | 0000011 | -    | 01011 | -      | 010   | 01101 | 000000000010       | 0x002B0323 |
| SLL r15, r11, r2| R    | 0110011 | 00010 | 01011 | 0000000 | 001   | 01111 | -                  | 0x000B5B33 |

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
