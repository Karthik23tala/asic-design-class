# ASIC Lab 2 Report (19/07/2024)

## Problem Statement
Compile the C code using Spike Simulator

### Task: Compile the C code using Spike Simulator instruction by instruction
Code:

```

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

'''
spike -d pk sum1ton.o
until pc 0 100b0
'''

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
