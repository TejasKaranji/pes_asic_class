# VLSI Design Course

## Objective

The objective of VLSI physical design for ASICs is to convert a logical RTL design into a physical layout suitable for fabrication. This process ensures the circuit's functionality aligns with design constraints, performance goals, and manufacturing standards.

## Table of Contents

### DAY 1

**Introduction to RISCV ISA and GNU Compiler Toolchain**
+ Introduction to Basic Keywords
+ Labwork for RISCV Toolchain
+ Integer Number Representation

### DAY 2
+ Application Binary Interface
+ Labwork using ABI Function Calls

## DAY 1 
<details>
<summary> Day 1 Lab Work</summary>

Writing C Program using Nano
```nano p1.c```
### Code 1: Sum of numbers from 1 to N:

Code to sum the numbers from 1 to N:

```c
#include<stdio.h>

int main(){
	int i, sum=0, n=111;
	for (i=1;i<=n; ++i) {
	sum +=i;
	}
	printf("Sum of numbers from 1 to %d is %d \n",n,sum);
	return 0;
}
```
To exit the editor, press `Ctrl + X` and `y` to save the file.

To compile for host system (Apple Silicon M2), using GCC
```
gcc p1.c -o p1.o
./p1.out

```
To compile for RISC-V

```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o p1.o p1.c
```
using ```ls``` to check that the compiled file exists:

***IMAGE***

To execute compiled code:
```
spike pk sum1ton.o
```

***IMAGE***

To show assembly code:
```
riscv64-unknown-elf-objdump -d sumton.o | less
```

With -O1 optimization:

***IMAGE***

With -OFast optimization:

***IMAGE***

***Using Spike to simulate and debug:***

To simulate:
```
spike -d pk p1.c
```
***IMAGE***

To Debug: 
```
spike -d pk p1.c
```
Press `ENTER` to show registers line by line
Press `q` to exit the debugger


Contents of the register are shown as in the image
***IMAGE***

### Integer number representation

- Range of Unsigned numbers : [0, (2^n)-1 ]
* Range of signed numbes : Positive : [0 , 2^(n-1)-1]
                         Negative : [-1 to 2^(n-1)]

Unsigned 64-Bit number:

```
#include <stdio.h>
#include <math.h>

int main(){
	unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
	unsigned long long int min = (unsigned long long int) (pow(2,64) *(-1));
	printf("lowest number represented by unsigned 64-bit integer is %llu\n",min);
	printf("highest number represented by unsigned 64-bit integer is %llu\n",max);
	return 0;
}
```

Output:

***IMAGE***

Signed 64-Bit Number

```
#include <stdio.h>
#include <math.h>

int main(){
	long long int max = (long long int) (pow(2,63) -1);
	long long int min = (long long int) (pow(2,63) *(-1));
	printf("lowest number represented by signed 64-bit integer is %lld\n",min);
	printf("highest number represented by signed 64-bit integer is %lld\n",max);
	return 0;
}
```
Output:
***IMAGE***
</details>

## DAY 2
<details>
	<summary> Day 2 Lab Work</summary>
 ### Lab work using ABI Function calls

Code:
``` c
 #include <stdio.h>
  
  extern int load(int x, int y);
  
  int main()
  {
    int result = 0;
    int count = 9;
    result = load(0x0, count+1);
    printf("Sum of numbers from 1 to 9 is %d\n", result);
  }
```
Compiled assembly file:

``` s
.section .text
.global load
.type load, @function
load:
add a4, a0, zero
add a2, a0, a1
add a3, a0, zero
loop:
add a4, a3, a4
addi a3, a3, 1
blt a3, a2, loop
add a0, a4, zero
ret
```


</details>
