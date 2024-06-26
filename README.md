# VSDSquadron Mini 

<details>
<summary>Task-1</summary>

+ In this program we will be using **RISC-V GNU Compiler Toolchain** to compile our c programs into risc v instructions. 
+ Then we will be using **Iverilog** to compile & simulate our verilog files. 
+ We will be using **GTKwave** to view the relevant waveforms obtained from verilog simulation.
+ Also we will be using **Yosys** to synthesis our RTL.
### Steps to install git

```
 sudo apt-get install git
 git clone https://github.com/kunalg123/vsdflow.git
 cd vsdflow
 chmod 777 opensource_eda_tool_install.sh
 ./opensource_eda_tool_install.sh
 ./vsdflow spi_slave_design_details.csv
 ./vsdflow picorv32_design_details.csv

```
![1](https://github.com/Amrutha3515/vsdmini/assets/150571663/7d1db528-055d-4aa6-a059-d648f1e5b220)

  ### steps to install RISC-V GNU-TOOL CHAIN
  
   + For installing the riscv-gnu tool chain on ubuntu computer first clone the following repository 
```
git clone https://github.com/riscv/riscv-gnu-toolchain   _where_u_want_to_clone_repo
  ```
```
sudo apt-get install autoconf automake autotools-dev curl python3 python3-pip libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bczlib1g-dev libexpat-dev ninja-build git cmake libglib2.0-dev libslirp-dev
 ``` 
#### The above code shows error : unable to locate the libslirp then use
```
sudo apt-get install autoconf automake autotools-dev curl python3 python3-pip libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat1-dev ninja-build git cmake libglib2.0-dev
cd directory_where_u_want_to_clone_repo
mkdir build
cd build
../configure --prefix=/absolute/path/where/u/want/to/keep/the/tools --enable-multilib
make
```
![4-1](https://github.com/Amrutha3515/vsdmini/assets/150571663/8cf8fb5a-3187-49ff-a44b-7a0ec1092150)

![5](https://github.com/Amrutha3515/vsdmini/assets/150571663/804ccf2e-e71a-4c24-87b0-923e061f03ab)

![7](https://github.com/Amrutha3515/vsdmini/assets/150571663/5889341e-e22f-4f75-af98-3f3981ceaf8e)
### Steps to install Yosys

```
sudo apt install yosys
```

You can verify the installation of yosys as shown below
![2-1](https://github.com/Amrutha3515/vsdmini/assets/150571663/3d291f38-f933-476b-8445-59b6690d12d3)


### Steps to install gcc 

```
 sudo apt install gcc
```
you can verify gcc installation as shown below
![2-2](https://github.com/Amrutha3515/vsdmini/assets/150571663/30a02b54-cb24-4ea6-b147-f85c85bcbf03)

## Steps to install Iverilog


```
 sudo apt-get update
 sudo apt-get -y install iverilog
```
![image](https://github.com/Amrutha3515/vsdmini/assets/150571663/8aa6bf8d-359f-47d8-be9d-ae0f808a5fbc)

The installation can be verified as shown below

![3-1](https://github.com/Amrutha3515/vsdmini/assets/150571663/3a485b68-6db4-4aff-aa47-f13193f454fb)


## Steps to install gtkwave

```
 sudo apt update
 sudo apt install gtkwave
```
![3-2](https://github.com/Amrutha3515/vsdmini/assets/150571663/55a2e7d5-f7e5-4a3f-ab1f-7cf1b3fef9e9)

# Compile a `c` Program Using Riscv Compiler


## Installing Leafpad Text Editor:


We will be using `leafpad` text editor to write our `c` program. The text editor can be installed as shown below (applicable for ubuntu 22.04 version).


```
sudo snap install leafpad 
```
## The Program:

Navigate to the home directory and create a new `.c` file in leafpad as shown below,

```
 cd 
 leafpad sum1ton.c &
```
write a `c program` and use the `save button`or use `ctrl + s`  to save the file. 

```
#include <stdio.h>
int main(){
    int sum = 0, i, n = 5;
    for (i=1; i<=n; ++i){
        sum += i;
    }
    printf("Sum of numbers from 1 to %d is %d \n", n, sum);
    return 0;
}

```

![8](https://github.com/Amrutha3515/vsdmini/assets/150571663/f23f70a9-04bf-4588-9a1b-f1e94de60a76)

## Compilation & Execution


Compile and run the program as below

```
 gcc program_name.c 
 ./a.out 
```
in my case it is sum1ton.c
![9](https://github.com/Amrutha3515/vsdmini/assets/150571663/970bafec-bd71-4319-8cd2-d82b524bd43a)

To view the code you have written and compile the program in riscv gcc compiler follow the below instruction.

```
cat program_name.c
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o program_name.o program_name.c
```
in my case it is sum1ton.o sum1ton.c

![14](https://github.com/Amrutha3515/vsdmini/assets/150571663/5120c785-1a2b-46ad-b493-d79e0d6a3f45)

Now open a new tab in terminal using `ctrl + shift + T` and follow below instruction to open the assembly code for the c program we had executed earlier.

```
riscv64-unknown-elf-objdump -d program_name.o
```
to search for `main()` section of our program below the below step,

```
riscv64-unknown-elf-objdump -d program_name.o | less
: /main
```

press `n` key to scrolldown & press `q` to quit.

![t-1](https://github.com/Amrutha3515/vsdmini/assets/150571663/dbfee5f1-1518-4b72-94e0-233e53a7176c)

The above image shows the `main()` section of my program. And as we can see there are 15 instructions in the `main()`. Address of each instruction can be seen. The address of the next instruction is `current address + 4 bytes`. <br>
<br> 
**Total Number of Instructions = (Address of the first instruction of the next instruction block -  Address of the first instruction of the current instruction block) / 4**
<br>
<br>
Therefore, Total number of instructions in main() =  (101c0 - 10184)/4 = (3c/4)<sub>**16**</sub>=(F)<sub>**16**</sub> = (**15**)<sub>**10**</sub>


## Role of -O1 & -Ofast: 

**-O** is a flag which directs the compiler to what extent it needs to optimize a given program, the optimization may be in terms of reducing binary size of the program while compramizing of the speed of execution or optimize the speed of exceution while increasing the binary program size. There are various levels of optimization,
<br>
**-O1** : This flag offers basic optimization, balances binary code size as well as speed. <br>
**-Ofast** : This flag offers aggressive optimizations. It prioritizes speed of execution over the size of compiled binary program size. <br>

Let us compile the same program with **-Ofast** flag.


```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o program_name.o program_name.c
# In a new terminal tab
riscv64-unknown-elf-objdump -d program_name.o | less
: /main
```
![t](https://github.com/Amrutha3515/vsdmini/assets/150571663/a39fcea4-6911-48e9-9acb-e50e19db3a6c)

The above image shows the `main()` section of my program. And as we can see there are 12 instructions in the `main()`. Address of each instruction can be seen. The address of the next instruction is `current address + 4 bytes`. <br>
<br> 
**Total Number of Instructions = (Address of the first instruction of the next instruction block -  Address of the first instruction of the current instruction block) / 4**
<br>
<br>
Therefore, Total number of instructions in main() =  (101e0 - 101bo)/4 = (30/4)<sub>**16**</sub>=(C)<sub>**16**</sub> = (**12**)<sub>**10**</sub>

we can observe that the Number of instructions is reduced with -Ofast

# Reference:
+ https://www.udemy.com/course/vsd-a-complete-guide-to-install-open-source-eda-tools/
+ https://github.com/riscv-collab/riscv-gnu-toolchain
+ https://1drv.ms/v/s!Ai4WW_jutengg7dbp9XlZXjJmxogBw?e=ycX4fO
+ https://1drv.ms/v/s!Ai4WW_jutenghrYpUsL_MLKJDSLVyg?e=gdA9TW
+ https://1drv.ms/v/s!Ai4WW_jutengifpPDmIttaabnLw5nw?e=ULYlur
</details>
<details >
 <summary>Task-2</summary>
 
 # Project: Traffic Flow Controller: Sequential Traffic Light Control System
 As my project is Traffic Flow Controller: Sequential Traffic Light Control System.
 Assuming we have two directions (North-South and East-West) with three LEDs (Red, Yellow, Green) for each direction.
 The sample C code for this project is 
 ```
#include <stdio.h>
#include <unistd.h> // for sleep function

// Define the traffic light states
typedef enum {
    RED,
    GREEN,
    YELLOW
} TrafficLightState;

// Define the directions
typedef enum {
    NS,  // North-South
    EW   // East-West
} Direction;

// Structure to represent a traffic light for a direction
typedef struct {
    TrafficLightState state;
    Direction direction;
} TrafficLight;

// Function to display the current state of the traffic light
void displayLight(TrafficLight *light) {
    const char *dir = (light->direction == NS) ? "North-South" : "East-West";
    switch (light->state) {
        case RED:
            printf("%s Direction: Red Light\n", dir);
            break;
        case GREEN:
            printf("%s Direction: Green Light\n", dir);
            break;
        case YELLOW:
            printf("%s Direction: Yellow Light\n", dir);
            break;
    }
}

int main() {
    TrafficLight nsLight = {RED, NS}; // North-South traffic light
    TrafficLight ewLight = {GREEN, EW}; // East-West traffic light

    while (1) {
        displayLight(&nsLight);
        displayLight(&ewLight);

        // Control the North-South traffic light
        switch (nsLight.state) {
            case RED:
                sleep(15);  // Red light for 15 seconds
                nsLight.state = GREEN;
                ewLight.state = RED;
                break;
            case GREEN:
                sleep(10);  // Green light for 10 seconds
                nsLight.state = YELLOW;
                break;
            case YELLOW:
                sleep(5);  // Yellow light for 5 seconds
                nsLight.state = RED;
                ewLight.state = GREEN;
                break;
        }

        // Control the East-West traffic light
        switch (ewLight.state) {
            case RED:
                // Red light duration is managed by North-South traffic light
                break;
            case GREEN:
                sleep(10);  // Green light for 10 seconds
                ewLight.state = YELLOW;
                break;
            case YELLOW:
                sleep(5);  // Yellow light for 5 seconds
                ewLight.state = RED;
                nsLight.state = GREEN;
                break;
        }
    }

    return 0;
}

```
To complie this code with RISC-V GCC. we can use the following commands on the ubuntu terminal .

```
leafpad demo.c
```
in my case demo is my file name , we can use any other names as you want and write the code that suitable for your selected project and save the file.

![VirtualBox_vsdmini_23_06_2024_20_54_14](https://github.com/Amrutha3515/vsdmini/assets/150571663/cb999ca8-9e44-4a9a-b23f-b2638d063c8e)
![VirtualBox_vsdmini_23_06_2024_20_53_42](https://github.com/Amrutha3515/vsdmini/assets/150571663/e6e353b7-1055-4219-80a8-3d571fd25987)
To compile andview the output of the code use the command
```
gcc demo.c
./a.out
```
![VirtualBox_vsdmini_23_06_2024_22_07_49](https://github.com/Amrutha3515/vsdmini/assets/150571663/30e9e206-026c-45b0-9037-ab7c3510f3c7)

As I Mentioned in the code i gave the delay of 15 seconds to 5 seconds varying according to the colour of the light to change the state.

 

https://github.com/Amrutha3515/vsdmini/assets/150571663/4631c83b-d29a-410c-86f6-b619a27bd191

From the video we can see that the no colour matches in the two directions. if the North- south direction is Red then the east -west direction is green. <br>

similarly, if the North-south direction is green then the east-west direction is red.

From this we can control the traffic light how and when to change the state of light. 
# Reference:
+ https://1drv.ms/v/s!Ai4WW_jutengifsvljZVcX_fvYEZtg?e=uMRUXd

</details>
<details >
 <summary>Task-3</summary>

 ### This Task is to do the spike simulation to the C code that we have done in Task-2.

 The code that is run using GCC Complier is again runs using RISCV and Spike. <br>
 As i started compiling using the RISC-V, i got an error with sleep function so i simply changed the sleep with delay function as follows
 ```
#include <stdio.h>
#include <unistd.h> // for sleep function

// Define the traffic light states
typedef enum {
   RED,
   GREEN,
   YELLOW
} TrafficLightState;

// Define the directions
typedef enum {
   NS,  // North-South
   EW   // East-West
} Direction;

// Structure to represent a traffic light for a direction
typedef struct {
   TrafficLightState state;
   Direction direction;
} TrafficLight;

// Function to display the current state of the traffic light
void displayLight(TrafficLight *light) {
   const char *dir = (light->direction == NS) ? "North-South" : "East-West";
   switch (light->state) {
       case RED:
           printf("%s Direction: Red Light\n", dir);
           break;
       case GREEN:
           printf("%s Direction: Green Light\n", dir);
           break;
       case YELLOW:
           printf("%s Direction: Yellow Light\n", dir);
           break;
   }
}
void delay(int seconds) {
    volatile unsigned int count;
    for (int i = 0; i < seconds * 1000000; i++) {
        count++;
    }
}
int main() {
   TrafficLight nsLight = {RED, NS}; // North-South traffic light
   TrafficLight ewLight = {GREEN, EW}; // East-West traffic light

   int cycles=5
   while (cycles > 0) {
       displayLight(&nsLight);
       displayLight(&ewLight);

       // Control the North-South traffic light
       switch (nsLight.state) {
           case RED:
               delay(15);  // Red light for 15 seconds
               nsLight.state = GREEN;
               ewLight.state = RED;
               break;
           case GREEN:
               delay(10);  // Green light for 10 seconds
               nsLight.state = YELLOW;
               break;
           case YELLOW:
               delay(5);  // Yellow light for 5 seconds
               nsLight.state = RED;
               ewLight.state = GREEN;
               break;
       }

       // Control the East-West traffic light
       switch (ewLight.state) {
           case RED:
               // Red light duration is managed by North-South traffic light
               break;
           case GREEN:
               sleep(10);  // Green light for 10 seconds
               ewLight.state = YELLOW;
               break;
           case YELLOW:
               sleep(5);  // Yellow light for 5 seconds
               ewLight.state = RED;
               nsLight.state = GREEN;
               break;
       }
    cycles--;
   }

   return 0;
}
```

<br> if you want to run the code continuously(infinite loop) then simply use **while(1)** as shown in task-2. for simulation purpose i am using the code to run for 5 cycles so i initialised the cycles and updated the code.<br>
for this code the output is same as in Task-2 except the no of times the code is executed.<br>
![image](https://github.com/Amrutha3515/vsdmini/assets/150571663/dae58623-0688-413e-85e9-30450cc64f77)

 now, we can proceed with riscv and spike simulations. To get the obj file using  riscv use 
 ```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o program_name.o program_name.c
```
<br> in my case it is demo1.c then open a new terminal and enter
```
riscv64-unknown-elf-objdump -d program_name.o
riscv64-unknown-elf-objdump -d program_name.o | less
```
![VirtualBox_vsdmini_25_06_2024_20_06_35](https://github.com/Amrutha3515/vsdmini/assets/150571663/b4679619-fe22-4142-9a35-c6587ebd5f4b)

 ### For spike simulation

 ```
spike pk program_name.o
```
<br>
To debugg the O file use

```
spike -d pk program_name.o
```
in my case it is demo1.o <br>


![VirtualBox_vsdmini_25_06_2024_21_39_53](https://github.com/Amrutha3515/vsdmini/assets/150571663/d28ab05d-f99a-489a-87a1-328e5f55dc7a)

### Now, Do the same steps using Ofast
![image](https://github.com/Amrutha3515/vsdmini/assets/150571663/8ae5e502-25d7-4a5a-9175-0f007b71e021)
![VirtualBox_vsdmini_25_06_2024_21_46_49](https://github.com/Amrutha3515/vsdmini/assets/150571663/a887d714-9e5c-4414-8e9d-4fe27d81bba0)
![VirtualBox_vsdmini_25_06_2024_21_46_27](https://github.com/Amrutha3515/vsdmini/assets/150571663/d4e4ee8f-f690-4fd8-ae26-9372817e33e9)

we can observe from this If you need your results to be very accurate and you want to be able to easily find and fix issues, use **-O1** and
Use **-Ofast** If you need the simulation to run as fast as possible and are okay with a small chance of less precise results.
# Reference:
+ https://1drv.ms/v/s!Ai4WW_jutengifwKKraoEerkS-LrnA?e=tGHh41
+ https://1drv.ms/v/s!Ai4WW_jutengg7dmZwxQmBY-JEGihg?e=A4ASgZ

</details>
<details >
 <summary>Task-4</summary>

 + Identify instruction type and exact 32-bit instruction code in the instruction type format.
  
  RV32I can be divided into six basic instruction formats. R-type instructions for register-register operations, an I-type instructions for immediate and load operations, and S-type instructions for store operations. B-type instructions for conditional branch operations. U-type instructions for long immediate and J-type instructions for unconditional jumps.

  ![WhatsApp Image 2024-02-22 at 17 29 06_b1e00065](https://github.com/Amrutha3515/RISC-V/assets/150571663/e06834ef-90e6-4b81-8ce4-9c720aff2562)
  
 +  add r6, r2, r1 	= R type instruction
+ sub r7, r1, r2	= R type instruction
+ and r8, r1, r3	= R type instruction
 + or r9, r2, r5	= R type instruction
+ xor r10, r1, r4	= R type instruction
+ slt r11, r2, r4	= R type instruction
+ addi r12, r4, 5	= I type instruction
+ sw r3, r1, 2		= S type instruction
+ lw r13, r1, 2		= s type instruction
+ beq r0, r0, 15	= B type instruction
+ bne r0, r1, 20	= B type instruction
+ sll r15, r1, r2	= R type instruction
+ srl r16, r14, r2	= R type instruction
 ``` 
add r6, r2, r1
0000000	00010	00001	000	00110	0110011

sub r7, r1, r2

0100000	00010	00001	000	00111	0110011

and r8, r1, r3
0000000	00011	00001	111	01000	0110011

or r9, r2, r5
0000000	00101	00010	110	01001	0110011

xor r10, r1, r4

0000000	00100	00001	100	01010	0110011
slt r11, r2, r4

0000000	00100	00010	010	01011	0110011

addi r12, r4, 5

0000000 000101   00100	000	01100	0010011

sw r3, r1, 2 

0000000	00011	00001	010	00010	0100011

lw r13, r1, 2

0000000 000010 00001 010 01101 0000011

beq r0, r0, 15

0000000 01111 00000 000 00000 1100011

bne r0, r1, 20

0000000 00101 00000 00001 001 00001 1100011

sll r15, r1, r2

0000000 00010 00001 001 01111 0110011

srl r16, r14, r2

0000000 00010 01110 101 10000 0110011
```
</details>
