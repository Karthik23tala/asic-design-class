# ASIC Lab 1 Report (16/07/2024)
## Problem Statement
Compile program for sum from 1 to n using c and riscv compiler

### Task 1: Compile the program in C using GCC

Code:
```

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

### Task 2: Compile the program in C using RISC-V compiler

Output: For O1


<img width="960" alt="image" src="https://github.com/user-attachments/assets/65a49cb5-4215-4d3b-936e-29cb8d9e4343">


Output: For OFast


<img width="960" alt="image" src="https://github.com/user-attachments/assets/80d9f58b-c041-41d9-bd8b-27b8de803d7b">

