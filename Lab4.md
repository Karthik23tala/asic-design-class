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
