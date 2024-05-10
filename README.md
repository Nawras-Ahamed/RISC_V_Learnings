# RISC_V_Learnings
A hobby repo for documenting my RISC-V learning progress


<details>
    <summary> TOOL INSTALLATION </summary>

1) install RISC-V GNU Toolchain 

2) install Yosys 

3) install iverilog 

4) install gtkwave

### CLONING RISC-V GNU TOOLCHAIN

```sudo apt install git-all```   # To install git

```sudo apt-get install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev``` *make sure to install the dependencies*

![gnu_dependencies](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/3a354063-2d87-44c5-8b54-e2f13d2b1965)

```git clone https://github.com/riscv/riscv-gnu-toolchain```

![gnu_toolchain_clone](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/760e3e42-8c07-4254-80a3-050e489ac42d)


## Create a opt dir
```mkdir /opt/riscv```  *try sudo incase of permission denial*

In my case I created a driectory ```mkdir riscv``` and ``` chmod 777 home/nawras/riscv ```

## Config and make inside the risc-v gnu toolchain dir 

```./configure --prefix=/opt/riscv```  

In my case ```./configure --prefix=/home/nawras/riscv```  

Then
```make``` **(Have patience)**

### Troubleshooting

**ERROR 1**: "gcc not found"
try ```sudo apt-get install build-essential```
see if gcc is in /usr/bin/

**ERROR 2**: "no acceptable c compiler found in $PATH"
Open the .bashrc by any editors like vim,emacs,nano,gedit ```nano ~/.bashrc``` 
Add the below line at the end of .bashrc and save it
```export PATH="$PATH:/usr/bin/gcc```

**ERROR 3**: Even after installing gcc g++ sometimes it shows 'gcc' command not found ,though it suggest to ```sudo apt install gcc``` which again will cause the same error. I figured this by ```ls```'ing the /usr/bin directory to find the gcc g++ cc to be in red text with black background indicates broken link or missing file.


Better purge it at **YOUR OWN RISK** and reinstall it again.
```sudo apt-get purge gcc```

or **REINSTALL** ```sudo apt-get install --reinstall gcc``` (didn't work for me)



### INSTALLING IVERILOG GTKWAVE & YOSYS

### YOSYS

```bash
git clone https://github.com/YosysHQ/yosys.git
cd yosys 
sudo apt-get install build-essential clang bison flex \libreadline-dev gawk tcl-dev libffi-dev git \ graphviz xdot pkg-config python3 libboost-system-dev\libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
make 
sudo make install
```

![yosys_make](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/e722e508-0802-4f50-9cb6-02e9c6bafe48)


![buildsuccess_yosys](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/5e10e5b8-19dd-4460-994f-2759e9b942b1)


### iVerilog

```
sudo apt-get install iverilog
```


### GTkWave
``` sudo apt-get install gtkwave ```

![iverilog_gtkwave](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/344a4225-c6bb-4728-a325-ac66d1621b28)

</details>


<details>
  <summary> RISC-V ISA AND INSTRUCTION ENCODING </summary>
  
  ### THE RISC-V ISA

[Instruction Set Manual](https://riscv.org/wp-content/uploads/2017/05/riscv-spec-v2.2.pdf)


The RISC-V ISA is defined as a base integer ISA, which must be present in any implementation, plus optional extensions to the base ISA.
The base integer instruction set, also known as the "RV32I" or "RV64I" instruction set, depending on the address space size, provides the core functionality required for general-purpose computing. 
It includes instructions for arithmetic, logical and control,memory access and manipulation <br>

The instruction Encoding of an operation in binary is known as its instruction format. RISC-V employs six core instruction formats, each encoded in a fixed-length 32-bit format for streamlined decoding and execution. These formats fall into six types:

R-type: For register-to-register operations like arithmetic and logical operations, utilizing three register operands. <br>
I-type: For short immediate operations involving arithmetic and logical operations with a 12-bit immediate value, employing two register operands. <br>
S-type: For store operations transferring data from a register to memory, involving two register operands and a 12-bit immediate value for memory address offset. <br>
B-type: For conditional branch operations directing control flow based on a condition, with two register operands and a 12-bit immediate value for branch target address. <br>
U-type: For operations with a 20-bit immediate(long) value, such as loading a constant or setting the upper 20 bits of a register. <br>
J-type: For unconditional jump operations transferring control to a different instruction unconditionally, with one register operand and a 20-bit immediate value for the jump target address. <br>

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/c5ee17d3-5017-41ba-bdbb-7c3acead31d8)


Instruction 1 : ``` add r6, r2, r1``` <br>
Instruction Type : **R-TYPE ARITHMETIC** <br>
Instruction Specification : Performs addition operation on the contents of registers r2 and r1 and stores the result in register r6. <br>
Instruction Encoding : | 0 0 0 0 0 0 0 | r1 | r2 | 0 0 0 | r6 | 0 1 1 0 0 1 1 |


Instruction 2 : ``` sub r7, r1, r2 ``` <br>
Instruction Type : **R-TYPE ARITHMETIC** <br>
Instruction Specification : Performs subtraction operation on the contents of registers r2 and r1 and stores the result in register r7. <br>
Instruction Encoding : | 0 1 0 0 0 0 0 | r2 | r1 | 0 0 0 | r7 | 0 1 1 0 0 1 1 |

Instruction 3 : ``` and r8, r1, r3``` <br>
Instruction Type : **R-TYPE LOGICAL** <br>
Instruction Specification : Performs bitwise AND operation between the contents of registers r1 and r3 and stores the result in register r8. <br>
Instruction Encoding : | 0 0 0 0 0 0 0 | r3 | rs1 | 1 1 1 | r8 | 0 1 1 0 0 1 1 |

Instruction 4 : ```or r9, r2, r5``` <br>
Instruction Type : **R-TYPE LOGICAL** <br>
Instruction Specification : Performs bitwise OR operation between the contents of registers r2 and r5 and stores the result in register r9. <br>
Instruction Encoding : | 0 0 0 0 0 0 0 | r5 | r2 | 1 1 0 | r9 | 0 1 1 0 0 1 1 |

Instruction 5 : ```xor r10, r1, r4``` <br>
Instruction Type : **R-TYPE LOGICAL** <br>
Instruction Specification : Performs bitwise XOR operation between the contents of registers r1 and r4 and stores the result in register r10. <br>
Instruction Encoding : | 0 0 0 0 0 0 0 | r4 | r1 | 1 0 0 | r10 | 0 1 1 0 0 1 1 |

Instruction 6 : ```slt r11, r2, r4``` <br>
Instruction Type : **R-TYPE LOGICAL** <br>
Instruction Specification : . It stands for "Set Less Than", and it compares the values in registers r2 and r4. If the value in r2 is less than the value in r4, it sets the value of r11 to 1; otherwise, it sets it to 0 <br>
Instruction Encoding : | 0 0 0 0 0 0 0 | rs4 | rs2 | 1 1 1 | r11 | 0 1 1 0 0 1 1 |

Instruction 7 : ```addi r12, r4, 5``` <br>
Instruction Type : **I-Type** <br>
Instruction Specification : adds the immediate value 5 to the value in register r4 and stores the result in register r12 <br>
Instruction Encoding :| 0 0 0 0 0 0 0 0 0 1 0 1 | rs4 | 0 0 0 | r12 |0 1 0 0 0 1 1 |

Instruction 8 : ```sw r3, r1, 2``` <br>
Instruction Type : **S-TYPE** <br>
Instruction Specification : stores the value from register r3 into the memory address formed by adding the immediate offset 2 to the value in register r1 <br>
Instruction Encoding : | 0 0 0 0 0 0 0 | r3 | r1 | 0 1 0 | 0 0 0 1 0 | 0 1 0 0 0 1 1 |

Instruction 9 : ```lw r13, r1, 2``` <br>
Instruction Type : **S-TYPE** <br>
Instruction Specification : used to load a 32-bit value from memory into a register <br>
Instruction Encoding : | 0 0 0 0 0 0 0 0 0 0 1 0| r1 | 0 1 0 | r13 | 0 0 0 0 0 1 1 |

Instruction 10 : ```beq r0, r0, 15``` <br>
Instruction Type : **B-TYPE** <br>
Instruction Specification :checks if the values in registers r0 and r0 are equal. Since r0 is typically the zero register (hardwired to zero) this always evaluates TRUE <br>
Instruction Encoding : | imm[12|10:5] | r0 | r0 | 0 0 0 | imm[4:1|11] | 1 1 0 0 0 1 1 |

Instruction 11 : ```bne r0, r1, 20``` <br>
Instruction Type : **B-TYPE** <br>
Instruction Specification :checks if the values in registers r0 and r1 are not equal If they are not equal the program will branch by adding the immediate offset of 20 to the PC <br>
Instruction Encoding : | imm[12|10:5] | r0 | r0 | 0 0 1 | imm[4:1|11] | 1 1 0 0 0 1 1 |

Instruction 12 : ```sll r15, r1, r2(2)``` <br>
Instruction Type : **R-TYPE** <br>
Instruction Specification :logical left shift on the value in register r1, shifting it left by a number of bits specified by the value in register r2 which is 2 in this case and stores the result in register r15 <br>
Instruction Encoding : | 0 0 0 0 0 0 0 | r1 | r2 | 0 0 1 | r15 | 0 1 1 0 0 1 1 |

Instruction 13 : ```srl r16, r14, r2(2)``` <br>
Instruction Type : **R-TYPE** <br>
Instruction Specification :logical right shift on the value in register r14, shifting it left by a number of bits specified by the value in register r2 which is 2 in this case and stores the result in register r16 <br>
Instruction Encoding : | 0 0 0 0 0 0 0 | r1 | r2 | 1 0 1 | r6 | 0 1 1 0 0 1 1 |


</details>


<details>
    <summary> COMPILING A C FILE & VIEWING THE OBJDUMP</summary>
   I just created a C program that sorts an array.

    #include <stdio.h>

    void main()

    {

        int i, j, a, n, x[30];

        printf("Enter the value of N \n");

        scanf("%d", &n);

        printf("Enter the numbers \n");

        for (i = 0; i < n; ++i)

            scanf("%d", &x[i]);

        for (i = 0; i < n; ++i)

        {
             for (j = i + 1; j < n; ++j)

            {

                if (x[i] > x[j])

                {

                   a =  x[i];

                    x[i] = x[j];

                    x[j] = a;

                }
            }

	}

	 for (i = 0; i < n; ++i)

            printf("%d \t", x[i]);

}



```riscv64-unknown-elf-gcc -o1 -o sorti.o sorti.```
![riscv_compile](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/b7e607e7-9cf9-4129-bbb0-401b85ef644a)


The obj file can be seen after running this 

while i can also see the riscv assembly 
```riscv64-unknown-elf-objdump -d sorti.o | less```

![main](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/54aab80e-5bb5-4448-a04b-f81d87b61810)


    
   
</details>
<details>
    <summary> Optimization of C code and SPIKE sim </summary>

**Why do we need Optimization?**
[Optimize-options in gcc](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)

Turning on optimization flags makes the compiler attempt to improve the performance and/or code size at the expense of compilation time and possibly the ability to debug the program

With ```-O1```, the compiler tries to reduce code size and execution time, without performing any optimizations.
Optimize even more. GCC performs nearly all supported optimizations that do not involve a space-speed tradeoff. As compared to -O, this option increases both compilation time and the performance of the generated code. 

With ```-Ofast``` it enables all -O3(optimize yet more) optimizations. It also enables optimizations that are not valid for all standard-compliant programs.

____________________
**ASM FOR SORTING AN ARRAY**

```bash
riscv64-unknown-elf-gcc -O1 -o sort.o sorti.c
riscv64-unknown-elf-objdump -d sort.o | less
```

```asm
00000000000101a4 <main>:
   101a4:       7135                    addi    sp,sp,-160
   101a6:       ed06                    sd      ra,152(sp)
   101a8:       00022537                lui     a0,0x22
   101ac:       24050513                addi    a0,a0,576 # 22240 <__clzdi2+0x3e>
   101b0:       496000ef                jal     10646 <puts>
   101b4:       18ec                    addi    a1,sp,124
   101b6:       00022537                lui     a0,0x22
   101ba:       25850513                addi    a0,a0,600 # 22258 <__clzdi2+0x56>
   101be:       490000ef                jal     1064e <scanf>
   101c2:       00022537                lui     a0,0x22
   101c6:       26050513                addi    a0,a0,608 # 22260 <__clzdi2+0x5e>
   101ca:       47c000ef                jal     10646 <puts>
   101ce:       57f6                    lw      a5,124(sp)
   101d0:       0af05063                blez    a5,10270 <main+0xcc>
   101d4:       e922                    sd      s0,144(sp)
   101d6:       e526                    sd      s1,136(sp)
   101d8:       e14a                    sd      s2,128(sp)
   101da:       848a                    mv      s1,sp
   101dc:       4401                    li      s0,0
   101de:       00022937                lui     s2,0x22
   101e2:       85a6                    mv      a1,s1
   101e4:       25890513                addi    a0,s2,600 # 22258 <__clzdi2+0x56>
   101e8:       466000ef                jal     1064e <scanf>
   101ec:       2405                    addiw   s0,s0,1
   101ee:       57f6                    lw      a5,124(sp)
   101f0:       0491                    addi    s1,s1,4
   101f2:       fef448e3                blt     s0,a5,101e2 <main+0x3e>
   101f6:       08f05063                blez    a5,10276 <main+0xd2>
   101fa:       004c                    addi    a1,sp,4
   101fc:       fff7889b                addiw   a7,a5,-1
   10200:       1882                    slli    a7,a7,0x20
   10202:       0208d893                srli    a7,a7,0x20
   10206:       8e3e                    mv      t3,a5
   10208:       4501                    li      a0,0
   1020a:       ffe7881b                addiw   a6,a5,-2
   1020e:       00810313                addi    t1,sp,8
   10212:       a00d                    j       10234 <main+0x90>
   10214:       0791                    addi    a5,a5,4
   10216:       00c78b63                beq     a5,a2,1022c <main+0x88>
   1021a:       ffc5a703                lw      a4,-4(a1)
   1021e:       4394                    lw      a3,0(a5)
   10220:       fee6dae3                bge     a3,a4,10214 <main+0x70>
   10224:       fed5ae23                sw      a3,-4(a1)
   10228:       c398                    sw      a4,0(a5)
   1022a:       b7ed                    j       10214 <main+0x70>
   1022c:       0505                    addi    a0,a0,1
   1022e:       0591                    addi    a1,a1,4
   10230:       01c50f63                beq     a0,t3,1024e <main+0xaa>
   10234:       0005079b                sext.w  a5,a0
   10238:       01150b63                beq     a0,a7,1024e <main+0xaa>
   1023c:       40f8063b                subw    a2,a6,a5
   10240:       1602                    slli    a2,a2,0x20
   10242:       9201                    srli    a2,a2,0x20
   10244:       962a                    add     a2,a2,a0
   10246:       060a                    slli    a2,a2,0x2
   10248:       961a                    add     a2,a2,t1
   1024a:       87ae                    mv      a5,a1
   1024c:       b7f9                    j       1021a <main+0x76>
   1024e:       848a                    mv      s1,sp
   10250:       4401                    li      s0,0
   10252:       00022937                lui     s2,0x22
   10256:       408c                    lw      a1,0(s1)
   10258:       27890513                addi    a0,s2,632 # 22278 <__clzdi2+0x76>
   1025c:       33c000ef                jal     10598 <printf>
   10260:       2405                    addiw   s0,s0,1
   10262:       0491                    addi    s1,s1,4
   10264:       57f6                    lw      a5,124(sp)
   10266:       fef448e3                blt     s0,a5,10256 <main+0xb2>
   1026a:       644a                    ld      s0,144(sp)
   1026c:       64aa                    ld      s1,136(sp)
   1026e:       690a                    ld      s2,128(sp)
   10270:       60ea                    ld      ra,152(sp)
   10272:       610d                    addi    sp,sp,160
   10274:       8082                    ret
   10276:       644a                    ld      s0,144(sp)
   10278:       64aa                    ld      s1,136(sp)
   1027a:       690a                    ld      s2,128(sp)
   1027c:       bfd5                    j       10270 <main+0xcc>
```
____________________

```bash
riscv64-unknown-elf-gcc -Ofast -o sort.o sorti.c
riscv64-unknown-elf-objdump -d sort.o | less
```

```asm
0000000000010104 <main>:
   10104:       00022537                lui     a0,0x22
   10108:       7171                    addi    sp,sp,-176
   1010a:       21050513                addi    a0,a0,528 # 22210 <__clzdi2+0x3c>
   1010e:       f506                    sd      ra,168(sp)
   10110:       e54e                    sd      s3,136(sp)
   10112:       506000ef                jal     10618 <puts>
   10116:       000229b7                lui     s3,0x22
   1011a:       004c                    addi    a1,sp,4
   1011c:       22898513                addi    a0,s3,552 # 22228 <__clzdi2+0x54>
   10120:       500000ef                jal     10620 <scanf>
   10124:       00022537                lui     a0,0x22
   10128:       23050513                addi    a0,a0,560 # 22230 <__clzdi2+0x5c>
   1012c:       4ec000ef                jal     10618 <puts>
   10130:       4792                    lw      a5,4(sp)
   10132:       06f05b63                blez    a5,101a8 <main+0xa4>
   10136:       f122                    sd      s0,160(sp)
   10138:       0020                    addi    s0,sp,8
   1013a:       ed26                    sd      s1,152(sp)
   1013c:       e94a                    sd      s2,144(sp)
   1013e:       4481                    li      s1,0
   10140:       8922                    mv      s2,s0
   10142:       85ca                    mv      a1,s2
   10144:       22898513                addi    a0,s3,552
   10148:       4d8000ef                jal     10620 <scanf>
   1014c:       4512                    lw      a0,4(sp)
   1014e:       2485                    addiw   s1,s1,1
   10150:       0911                    addi    s2,s2,4
   10152:       fea4c8e3                blt     s1,a0,10142 <main+0x3e>
   10156:       04a05663                blez    a0,101a2 <main+0x9e>
   1015a:       4785                    li      a5,1
   1015c:       02f50663                beq     a0,a5,10188 <main+0x84>
   10160:       006c                    addi    a1,sp,12
   10162:       4805                    li      a6,1
   10164:       87ae                    mv      a5,a1
   10166:       8742                    mv      a4,a6
   10168:       4390                    lw      a2,0(a5)
   1016a:       ffc5a683                lw      a3,-4(a1)
   1016e:       2705                    addiw   a4,a4,1
   10170:       00d65563                bge     a2,a3,1017a <main+0x76>
   10174:       fec5ae23                sw      a2,-4(a1)
   10178:       c394                    sw      a3,0(a5)
   1017a:       0791                    addi    a5,a5,4
   1017c:       fea746e3                blt     a4,a0,10168 <main+0x64>
   10180:       2805                    addiw   a6,a6,1
   10182:       0591                    addi    a1,a1,4
   10184:       ff0510e3                bne     a0,a6,10164 <main+0x60>
   10188:       4481                    li      s1,0
   1018a:       00022937                lui     s2,0x22
   1018e:       400c                    lw      a1,0(s0)
   10190:       24890513                addi    a0,s2,584 # 22248 <__clzdi2+0x74>
   10194:       2485                    addiw   s1,s1,1
   10196:       3d4000ef                jal     1056a <printf>
   1019a:       4792                    lw      a5,4(sp)
   1019c:       0411                    addi    s0,s0,4
   1019e:       fef4c8e3                blt     s1,a5,1018e <main+0x8a>
   101a2:       740a                    ld      s0,160(sp)
   101a4:       64ea                    ld      s1,152(sp)
   101a6:       694a                    ld      s2,144(sp)
   101a8:       70aa                    ld      ra,168(sp)
   101aa:       69aa                    ld      s3,136(sp)
   101ac:       614d                    addi    sp,sp,176
   101ae:       8082                    ret
```
______________________
**INSTALLING SPIKE**

[SPIKE RISCV ISA SIM](https://github.com/riscv-software-src/riscv-isa-sim)
  
  ```bash
 git clone https://github.com/riscv-software-src/riscv-isa-sim.git
 sudo apt-get install device-tree-compiler libboost-regex-dev
 mkdir build
 cd build
 ../configure --prefix=/home/nawras/riscv
 make
 sudo make install

 ```
The ```--prefix=/home/nawras/riscv``` is where the path is set to.

**INSTALLING RISCV PROXY KERNEL (PK)**

```bash
git clone https://github.com/riscv-software-src/riscv-pk.git
mkdir build
cd build
../configure --prefix=/home/nawras/riscv --host=riscv64-unknown-elf
make
make install
```

[**TROUBLESHOOT 1 -  HOST COMPILER , riscv-unknown-elf & PATH**](https://github.com/riscv-software-src/riscv-pk/issues/204)

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/29b8b342-f2fd-45f4-9392-8227509e8fb9)

**ERROR 2** 
![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/bd31c5d7-1b43-4082-9db3-fcd12714ac29)

[**TROUBLESHOOT 2 - Error: unrecognized opcode fence.i, extension zifencei required**](https://github.com/riscv-software-src/riscv-pk/issues/260) <br>

Looks like the fence instruction is needed and I have to build a seperate riscv gnu toolchain for this by 
```bash
cd riscv-gnu-toolchain
mkdir build
cd build
../configure --prefix=/home/nawras/riscv --with-arch=rv64gc_zfencei --with-abi=lp64d
make
```

NOW I HAVE TO AGAIN CONFIGURE AND BUILD THE PROXY KERNEL 
```bash
git clone https://github.com/riscv-software-src/riscv-pk.git
mkdir build
cd build
../configure --prefix=/home/nawras/riscv --host=riscv64-unknown-elf
make
make install
```
*I HAD  NO ERRORS AFTER THESE STEPS*
![PK SUCCESS](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/5e67e505-9f26-4d6d-a538-5427e6701266)
___________________________

**OUTPUT WITH SPIKE PK**

![spike_pk_o1](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/d435db5a-bcc8-41fb-85f4-1599983dbad1)

**OUTPUT WITH GCC**

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/6f847479-1a6b-4551-9366-b5d473e0f003)

</details>


<details>
  <summary> FUNCTIONAL VERIFICATION OF A RV32I core </summary>

[Original Source](https://github.com/vinayrayapati/rv32i)


![instructions_half-done](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/a796cca4-a63c-4953-91bd-8e4ff8a34577)


<details>
	<summary> ADD r6, r2, r1</summary>
 			
 
	Instruction 1 :  add r6, r2, r1
	Instruction Type : R-TYPE ARITHMETIC
	Instruction Specification : Performs addition operation on the contents of registers r2 and r1 and stores the result in register r6.
	Instruction Encoding : | 0 0 0 0 0 0 0 | r1 | r2 | 0 0 0 | r6 | 0 1 1 0 0 1 1 |

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/b827d936-7a05-4868-b792-bcc92c4085c1)


 Can see that ALU out contents are being written back to the reg file after two clock cycles and the opcode decode in the above waveform
</details>


<details>
<summary> SUB r7, r1, r2</summary>
 			
 
	Instruction 2 : sub r7, r1, r2
	Instruction Type : R-TYPE ARITHMETIC
	Instruction Specification : Performs subtraction operation on the contents of registers r2 and r1 and stores the result in register r7.
	Instruction Encoding : | 0 1 0 0 0 0 0 | r2 | r1 | 0 0 0 | r7 | 0 1 1 0 0 1 1 |

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/f9b98cd3-18db-4d92-960b-b25d7a14fcce)


Here the result is FFFFFFFF which is -1 in signed decimal representation as we have r2 = 2 and r1 = 1.
 
 
</details>


<details>
<summary> AND r8, r1, r3</summary>
 			
 
	Instruction 3 :  and r8, r1, r3
	Instruction Type : R-TYPE LOGICAL
	Instruction Specification : Performs bitwise AND operation between the contents of registers r1 and r3 and stores the result in register r8.
	Instruction Encoding : | 0 0 0 0 0 0 0 | r3 | rs1 | 1 1 1 | r8 | 0 1 1 0 0 1 1 |
![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/9487ea91-c78f-449c-93e9-79f840de0580)


r1 = 1  (01)
r3 = 3  (11)
So r8 = (01) on `and` operation

</details>

<details>
<summary> OR r9, r2, r5</summary>
 			
 
	Instruction 4 : or r9, r2, r5
	Instruction Type : R-TYPE LOGICAL
	Instruction Specification : Performs bitwise OR operation between the contents of registers r2 and r5 and stores the result in register r9.
	Instruction Encoding : | 0 0 0 0 0 0 0 | r5 | r2 | 1 1 0 | r9 | 0 1 1 0 0 1 1 |

r2 = 2  (010)
r5 = 5  (101)
So r9 = (111) on `or` operation

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/e8eadbc3-01e3-480c-8a5d-4cd1f79cad5d)


</details>

<details>
<summary> XOR r10, r1, r4</summary>
 			
 
	Instruction 5 : xor r10, r1, r4
	Instruction Type : R-TYPE LOGICAL
	Instruction Specification : Performs bitwise XOR operation between the contents of registers r1 and r4 and stores the result in register r10.
	Instruction Encoding : | 0 0 0 0 0 0 0 | r4 | r1 | 1 0 0 | r10 | 0 1 1 0 0 1 1 |
r1 = 1  (001)
r4 = 4  (100)
So r10 = (101) on `xor` operation

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/1df1b654-b160-48f3-8154-24af2437d493)


</details>

<details>
<summary> SLT r11, r2, r4</summary>
 			
	Instruction 6 : slt r11, r2, r4
	Instruction Type : R-TYPE LOGICAL
	Instruction Specification : . It stands for "Set Less Than", and it compares the values in registers r2 and r4. If the value in r2 is less than the value in r4, it sets the value of r11 to 1; otherwise, it sets it to 0
	Instruction Encoding : | 0 0 0 0 0 0 0 | rs4 | rs2 | 1 1 1 | r11 | 0 1 1 0 0 1 1 |

r2 = 2 (010)
r4 = 4 (100)

r11 = 1 since r2 value is less than r4 value

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/81414194-c6e8-4508-872a-1102a466a110)

</details>

<details>
<summary> ADDI r12, r4, 5</summary>
 			
	Instruction 7 : addi r12, r4, 5
	Instruction Type : I-Type
	Instruction Specification : adds the immediate value 5 to the value in register r4 and stores the result in register r12
	Instruction Encoding :| 0 0 0 0 0 0 0 0 0 1 0 1 | rs4 | 0 0 0 | r12 |0 1 0 0 0 1 1 |

imm_value = 5
r4 = 4

r12 -> (r4)+5 
r12 = 9

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/1e847afa-efbe-4020-b437-9c68f110411d)

can see the `ID_EX_IMMEDIATE SIGNAL` @48ms getting loaded with 00000005

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/4d4276aa-1a02-42ab-b3e9-ac45c76ea4cb)


</details>

<details>
<summary> SW r3, r1, 2</summary>
	
	Instruction 8 : sw r3, r1, 2
	Instruction Type : S-TYPE
	Instruction Specification : stores the value from register r3 into the memory address formed by adding the immediate offset 2 to the value in register r1
	Instruction Encoding : | 0 0 0 0 0 0 0 | r3 | r1 | 0 1 0 | 0 0 0 1 0 | 0 1 0 0 0 1 1 |

imm_value (offset)  = 2
r1 = 1 
r3 = 3

effective_addr = 3 
MEM[effective_addr] -> (r3)


![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/cf132ff6-c383-426c-8188-bab9e2bf1d2c)

Can see the value `3` being written @72 ms into the memory 

</details>

<details>
<summary> LW r13, r1, 2</summary>
	
	Instruction 9 : lw r13, r1, 2
	Instruction Type : S-TYPE
	Instruction Specification : used to load a 32-bit value from memory into a register
	Instruction Encoding : | 0 0 0 0 0 0 0 0 0 0 1 0| r1 | 0 1 0 | r13 | 0 0 0 0 0 1 1 |

imm_value (offset)  = 2
r1 = 1 
effective_addr = 3 
(MEM[effective_addr]) = 3 <br>

r13 = 3 (from the memory)

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/fb45c8bc-84cf-4d2b-a13c-5682031b8cba)


Can see the value `3` being written from memory to the register @80ms since the prev instruction was of storing the data at the same location and now its loading it into a register we dont find to see any change in the signals(held @ 3)

</details>

<details>
<summary> BEQ r0, r0, 15</summary>
	
	Instruction 10 : beq r0, r0, 15
	Instruction Type : B-TYPE
	Instruction Specification :checks if the values in registers r0 and r0 are equal. Since r0 is typically the zero register (hardwired to zero) this always evaluates TRUE
	Instruction Encoding : | imm[12|10:5] | r0 | r0 | 0 0 0 | imm[4:1|11] | 1 1 0 0 0 1 1 |

The expression is always true, so the `BR_EN= 1` @72ms 

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/ac114bc0-20bf-4053-b46c-17571a884ee9)



In this final stage, the branch target address is determined, but no write-back operation occurs.


Instead, the branch target address is passed to the next stage of the pipeline to update the program counter which can be seen at the cursor in the picture @78ms (see the PROGRAM COUNTER `NPC`)
0C to 19 (19-C = 13 in decimal)this happens due to the imm_value = 15 which becomes the target address and gets updated to the Program Counter. 
![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/7640f390-de5a-45c8-9cf1-80cdd0eaadb3)



So we can say that the PC has jumped as the Branch conditions are met.

We can conclude that after this their would be no instructions (as per the verilog code) to be exceuted so we get no Writeback operations to take place, But since it's a 5-Stage pipelined processor The other instructions might have been done halfway till Execute stage which can be seen clearly at the `EX_MEM_ALUOUT`
![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/03487ba4-69e3-4897-bcf2-1d5e28f7d5d7) 

</details>

<details>

<summary> ADD r14,r2,r2</summary>
	
	Instruction 11:  add r14, r2, r2
	Instruction Type : R-TYPE ARITHMETIC
	Instruction Specification : Performs addition operation on the contents of registers r2 and r2 and stores the result in register r14.
	Instruction Encoding : | 0 0 0 0 0 0 0 | r2 | r2 | 0 0 0 | r14 | 0 1 1 0 0 1 1 |

`MEM[25] <= 32'h00210700;         //add r14,r2,r2.(i11)`

r2 = 2
r14 = 4

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/4234f260-e79c-4a31-99d3-3b937bee2162)

</details>
____________________________

### Since there were other instructions which were commented out


<details> 

 
<summary> BNE r0, r1, 20</summary>
	
	Instruction 11 : bne r0, r1, 20
	Instruction Type : B-TYPE
	Instruction Specification :checks if the values in registers r0 and r1 are not equal If they are not equal the program will branch by adding the immediate offset of 20 to the PC
	Instruction Encoding : | imm[12|10:5] | r0 | r0 | 0 0 1 | imm[4:1|11] | 1 1 0 0 0 1 1 |


![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/febefaed-18c5-42cc-b516-c9168a6269d8)


WE can see that `BR_EN=1` @108ms

In this final stage, the branch target address is determined, but no write-back operation occurs.
Instead, the branch target address is passed to the next stage of the pipeline to update the program counter which can be seen at the cursor in the picture @84ms (see the PROGRAM COUNTER `NPC`)
1E to 30 (30-1E = 18 in decimal) this happens due to the imm_value = 20 which becomes the target address and gets updated to the Program Counter. 


So we can say that the PC has jumped  as the Branch conditions are met.

</details>

<details>
	
<summary> SLL r15, r1, r2(2)</summary>
	
	Instruction 12 : sll r15, r1, r2(2)
	Instruction Type : R-TYPE
	Instruction Specification :logical left shift on the value in register r1, shifting it left by a number of bits specified by the value in register r2 which is 2 in this case and stores the result in register r15
	Instruction Encoding : | 0 0 0 0 0 0 0 | r1 | r2 | 0 0 1 | r15 | 0 1 1 0 0 1 1 |


![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/f1b7801d-65d6-41ef-8611-973eea7fff04)

Since r1 = 001
on left shift of 2 we get 100 which is 4 in decimal hence it's stored at r15 and can be seen at the cursor

</details>

<details>
	
<summary> SRL r16, r14, r2(2)</summary>
	
	Instruction 13 : srl r16, r14, r2(2)
	Instruction Type : R-TYPE
	Instruction Specification :logical right shift on the value in register r14, shifting it left by a number of bits specified by the value in register r2 which is 2 in this case and stores the result in register r16
	Instruction Encoding : | 0 0 0 0 0 0 0 | r1 | r2 | 1 0 1 | r6 | 0 1 1 0 0 1 1 |


Since r14 = 4 , which was updated by the add instruction @150ms

On right shift to 2 we get r14 as 001 which is then stored in r16 register.

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/087d6488-ac4e-468f-b520-56194c803ae2)

</details>

</details>

<details>
	<summary> GATE LEVEL SIMULATION </summary>

```bash
	cd rv32i
	yosys
	read_verilog iiitb_rv32i.v
	synth -top iiitb_rv32i
```
 ![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/19bb02d3-dc62-48be-a78f-91ca0fcf3056)

 ```bash
dfflibmap -liberty /home/nawras/rv32i/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/fe07dea0-5429-40f1-9097-91f87cbb4fdf)

```bash
proc; opt
```
![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/0fb3d80a-a4de-44e3-8417-dfd7a47d9a8f)

```bash
abc -liberty /home/nawras/rv32i/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime,{D};strash;dch,-f;map,-M,1,{D}
```

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/12ec4822-dfe2-4759-ad1b-4f430bb9de40)

```bash
clean
flatten
```

```bash
write_verilog -noattr iiitb_rv32i_synth.v
stat

=== iiitb_rv32i ===

   Number of wires:               5307
   Number of wire bits:           8700
   Number of public wires:          97
   Number of public wire bits:    2994
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:               7340
     sky130_fd_sc_hd__a2111oi_0      9
     sky130_fd_sc_hd__a211oi_1      20
     sky130_fd_sc_hd__a21boi_0       5
     sky130_fd_sc_hd__a21o_1        50
     sky130_fd_sc_hd__a21oi_1     1563
     sky130_fd_sc_hd__a221o_1        5
     sky130_fd_sc_hd__a221oi_1      16
     sky130_fd_sc_hd__a222oi_1     194
     sky130_fd_sc_hd__a22o_1         3
     sky130_fd_sc_hd__a22oi_1      499
     sky130_fd_sc_hd__a2bb2oi_1      1
     sky130_fd_sc_hd__a311o_1        1
     sky130_fd_sc_hd__a311oi_1       7
     sky130_fd_sc_hd__a31o_1         2
     sky130_fd_sc_hd__a31oi_1       17
     sky130_fd_sc_hd__a32oi_1       35
     sky130_fd_sc_hd__a41oi_1        1
     sky130_fd_sc_hd__and2_0        72
     sky130_fd_sc_hd__and3_1        53
     sky130_fd_sc_hd__and4_1         2
     sky130_fd_sc_hd__clkinv_1      52
     sky130_fd_sc_hd__dfrtp_1       32
     sky130_fd_sc_hd__dfxtp_1     1618
     sky130_fd_sc_hd__maj3_1        15
     sky130_fd_sc_hd__mux2_1        43
     sky130_fd_sc_hd__mux2i_1       33
     sky130_fd_sc_hd__mux4_2         8
     sky130_fd_sc_hd__nand2_1      572
     sky130_fd_sc_hd__nand2b_1      44
     sky130_fd_sc_hd__nand3_1       75
     sky130_fd_sc_hd__nand3b_1       3
     sky130_fd_sc_hd__nand4_1       12
     sky130_fd_sc_hd__nand4b_1       1
     sky130_fd_sc_hd__nor2_1      1272
     sky130_fd_sc_hd__nor2b_1       19
     sky130_fd_sc_hd__nor3_1        41
     sky130_fd_sc_hd__nor3b_1        4
     sky130_fd_sc_hd__nor4_1        82
     sky130_fd_sc_hd__nor4b_1        3
     sky130_fd_sc_hd__nor4bb_1       1
     sky130_fd_sc_hd__o2111ai_1      5
     sky130_fd_sc_hd__o211a_1        4
     sky130_fd_sc_hd__o211ai_1      27
     sky130_fd_sc_hd__o21a_1        18
     sky130_fd_sc_hd__o21ai_0      501
     sky130_fd_sc_hd__o21bai_1       1
     sky130_fd_sc_hd__o221a_1        3
     sky130_fd_sc_hd__o221ai_1      16
     sky130_fd_sc_hd__o22a_1         5
     sky130_fd_sc_hd__o22ai_1       42
     sky130_fd_sc_hd__o2bb2ai_1      1
     sky130_fd_sc_hd__o311a_1        3
     sky130_fd_sc_hd__o311ai_0       4
     sky130_fd_sc_hd__o31a_1         2
     sky130_fd_sc_hd__o31ai_1       19
     sky130_fd_sc_hd__o32a_1         2
     sky130_fd_sc_hd__o32ai_1       22
     sky130_fd_sc_hd__or2_0         33
     sky130_fd_sc_hd__or3_1         29
     sky130_fd_sc_hd__or3b_1         4
     sky130_fd_sc_hd__or4_1          4
     sky130_fd_sc_hd__xnor2_1       73
     sky130_fd_sc_hd__xor2_1        37
```

TROUBLESHOOT 1 :

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/14d3982c-ba57-4129-9921-3ac84896636d)

Resolved by Referring to https://github.com/The-OpenROAD-Project/OpenLane/issues/518#issuecomment-894181871

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/9f487701-5861-40db-9449-6e946a10fa40)



**add r6, r2, r1**

![ADD_GLS](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/ed35181f-4d92-4aef-b2d6-f86bfef58665)

**sub r7, r1, r2**

![SUB_GLS](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/d739eeda-1041-4c31-9555-5d2554117158)

**and r8, r1, r3**

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/e16297be-ed7e-4a21-97a2-0679ebb3fa37)

**or r9, r2, r5**

![Screenshot from 2024-03-14 19-17-07](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/73597dc6-3889-41c9-8d4f-d4475ff3eb14)

**xor r10, r1, r4**

![Screenshot from 2024-03-14 19-17-20](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/27d367a6-ae36-44e7-a038-69c25a4f1d0d)

**slt r11, r2, r4**

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/cb982257-0faf-4135-9495-19c3ab78ce4d)

**addi r12, r4, 5**

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/8539a8e6-177a-4cda-91fa-fccd0dfed61e)

**sw r3, r1, 2**

![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/d6ef0f6a-af45-4a92-b27f-05d0bb0dfb96)

**add r14,r2,r2**

![Screenshot from 2024-03-14 21-52-15](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/9aef3522-8e1d-4133-9f90-f75c949236ec)


**NETLIST**
![image](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/b7b2d5f8-7e17-481b-922d-6936963eff68)


**GLS**


![Screenshot_20240317-092741](https://github.com/Nawras-Ahamed/VSD_Squadron_mini_Research/assets/50738659/51ddff75-9bec-48a9-8559-c6afc925dda3)


</details>
