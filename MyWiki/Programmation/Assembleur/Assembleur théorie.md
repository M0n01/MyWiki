#assembleur #langages #programmation 

==Langage Compilé== = Le code est traduit par la machine en langage machine et c'est ce code machine qui sera exécuter. Plus rapide.

==Langage Interprété== = Le code est traduit à chaque exécution puis exécuter par la machine. Donc moins rapide.

Première ligne pour un script python par exemple : #!/bin/python
Pour dire comment interpréter le script

Lorsque le code est compilé, il est traduit en instructions d'assembleur pour le processeur pour lequel il est compilé, qui sont ensuite assemblées en code machine à exécuter sur le processeur.

![[Pasted image 20240106150907.png]]

>[!Note]
>With multi-platform languages, like `Java`, the code is compiled into a Java Bytecode, which is the same for all processors/systems, and is then compiled to machine code by the local Java Runtime environment. _This is what makes Java relatively slower than other languages like C++ that compile directly into machine code. Languages like C++ are more suitable for processor intensive applications like games._

# Computer architecture

This architecture executes machine code to perform specific algorithms. It mainly consists of the following elements:

- Central Processing Unit (CPU)
- Memory Unit
- Input/Output Devices
    - Mass Storage Unit
    - Keyboard
    - Display

**Assembly languages mainly work with the CPU and memory.**

## CPU Architecture

The CPU itself consists of three main components:

- Control Unit (CU)
- Arithmetic/Logic Unit (ALU)
- Registers

![[Pasted image 20240106152100.png]]
- `Control Unit` (CU), is in charge of moving and controlling data,
- `Arithmetic/Logic Unit` (ALU), is in charge of performing various arithmetics and logical calculations as requested by a program through the assembly instructions.

The manner in which and how efficiently a CPU processes its instructions depends on its **`Instruction Set Architecture` (ISA)**.

There are multiple ISA's : 
- `RISC` architecture is based on processing more simple instructions, which takes more cycles, but each cycle is shorter and takes less power. 
- `CISC` architecture is based on fewer, more complex instructions, which can finish the requested instructions in fewer cycles, but each instruction takes more time and power to be processed.

#### Clock Speed & Clock Cycle

Each CPU has a clock speed that indicates its overall speed. Every tick of the clock runs a clock cycle that processes a basic instruction, such as fetching an address or storing an address. Specifically, this is done by the CU or ALU.

The frequency in which the cycles occur is counted is cycles per second (`Hertz`). If a CPU has a speed of 3.0 GHz, it can run 3 billion cycles every second (per core).

![[Pasted image 20240106163907.png]]

Modern processors have a multi-core design, allowing them to have multiple cycles at the same time.

#### Instruction Cycle

An `Instruction Cycle` is the cycle it takes the CPU to process a single machine instruction.

![[Pasted image 20240106164034.png]]

|**Instruction**|**Description**|
|---|---|
|`1.Fetch` |Takes the next instruction's address from the `Instruction Address Register` (IAR), which tells it where the next instruction is located.|
|`2.Decode` |Takes the instruction from the IAR, and decodes it from binary to see what is required to be executed.|
|`3.Execute` |Fetch instruction operands from register/memory, and process the instruction in the `ALU` or `CU`.|
|`4.Store` |Store the new value in the destination operand.|

>[!Info]
>All of the stages in the instruction cycle are carried out by the Control Unit, except when arithmetic instructions need to be executed "add, sub, ..etc", which are executed by the ALU.

Each Instruction Cycle takes multiple clock cycles to finish, depending on the CPU architecture and the complexity of the instruction. Once a single instruction cycle ends, the CU increments to the next instruction and runs the same cycle on it, and so on.

>[!Exemple]
>For example, if we were to execute the assembly instruction `add rax, 1`, it would run through an instruction cycle:
>1. Fetch the instruction from the `rip` register, `48 83 C0 01` (in binary).
>2. Decode '`48 83 C0 01`' to know it needs to perform an `add` of `1` to the value at `rax`.
>3. Get the current value at `rax` (by `CU`), add `1` to it (by the `ALU`).
>4. Store the new value back to `rax`.

Modern processors can process multiple instructions in parallel by having multiple instruction/clock cycles running at the same time. This is made possible by having a multi-thread and multi-core design.

## Memory 

A computer's memory is where the `temporary` data and instructions of currently running programs are located. There are two main types of memory:

1. `Cache`
2. `Random Access Memory (RAM)`

#### Cache

Cache memory is usually located within the CPU itself and hence is extremely fast compared to RAM, as it runs at the same clock speed as the CPU. However, it is very limited in size and very sophisticated, and expensive to manufacture due to it being so close to the CPU core.

There are usually three levels of cache memory, depending on their closeness to the CPU core:

|**Level**|**Description**|
|---|---|
|`Level 1 Cache`|Usually in kilobytes, the fastest memory available, located in each CPU core. (Only registers are faster.)|
|`Level 2 Cache`|Usually in megabytes, extremely fast (but slower than L1), shared between all CPU cores.|
|`Level 3 Cache`|Usually in megabytes (larger than L2), faster than RAM but slower than L1/L2. (Not all CPUs use L3.)|
#### RAM

- much larger than cache memory
- located far away from the CPU cores 
- much slower than cache memory.

Accessing data from RAM addresses takes many more instructions.

For example, retrieving an instruction from the registers takes only one clock cycle, and retrieving it from the L1 cache takes a few cycles, while retrieving it from RAM takes around 200 cycles. When this is done billions of times a second, it makes a massive difference in the overall execution speed.

As we can see, the RAM is split into four main `segments`:

![[Pasted image 20240106153204.png]]

|Segment|Description|
|---|---|
|`Stack`|Has a Last-in First-out (LIFO) design and is fixed in size. Data in it can only be accessed in a specific order by push-ing and pop-ing data.|
|`Heap`|Has a hierarchical design and is therefore much larger and more versatile in storing data, as data can be stored and retrieved in any order. However, this makes the heap slower than the Stack.|
|`Data`|Has two parts: `Data`, which is used to hold variables, and `.bss`, which is used to hold unassigned variables (i.e., buffer memory for later allocation).|
|`Text`|Main assembly instructions are loaded into this segment to be fetched and executed by the CPU.|
This segmentation applies to the entire RAM, `each application is allocated its Virtual Memory when it is run`. 
This means that each application would have its own `stack`, `heap`, `data`, and `text` segments.

### IO/Storage

The Input/Output devices, like the keyboard, the screen, or the long-term storage unit, also known as Secondary Memory. The processor can access and control IO devices using `Bus Interfaces`, which act as 'highways' to transfer data and addresses, using electrical charges for binary data.

The storage unit is the slowest to access. First, because they are the farthest away from the CPU, accessing them through bus interfaces like SATA or USB takes much longer to store and retrieve the data. They are also slower in their design to allow more data storage. Αs long as there is more data to go through, they will be slower.

### Speed

As we can see from the above, the further away a component is from the CPU core, the slower it is. Also, the more data it can hold, the slower it is, as it simply has to go through more to fetch the data.

|Component|Speed|Size|
|---|---|---|
|`Registers`|Fastest|Bytes|
|`L1 Cache`|Fastest, other than Registers|Kilobytes|
|`L2 Cache`|Very fast|Megabytes|
|`L3 Cache`|Fast, but slower than the above|Megabytes|
|`RAM`|Much slower than all of the above|Gigabytes-Terabytes|
|`Storage`|Slowest|Terabytes and more|




