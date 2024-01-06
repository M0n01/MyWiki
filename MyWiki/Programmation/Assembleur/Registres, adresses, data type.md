#registres #adresses #datatype #type #data 

## Registers

Each CPU core has a set of registers. The registers are the fastest components in any computer, as they are built within the CPU core. However, registers are very limited in size and can only hold a few bytes of data at a time. 
There are many registers in the x86 architecture, but we will only focus on the ones necessary for learning basic Assembly and essential for future binary exploitation.

There are two main types of registers : **`Data Registers`** and **`Pointer Registers`**.

|**Data Registers**|**Pointer Registers**|
|---|---|
|`rax`|`rbp`|
|`rbx`|`rsp`|
|`rcx`|`rip`|
|`rdx`||
|`r8`||
|`r9`||
|`r10`|

- `Data Registers` - are usually used for storing instructions/syscall arguments. The primary data registers are: `rax`, `rbx`, `rcx`, and `rdx`. The `rdi` and `rsi` registers also exist and are usually used for the instruction `destination` and `source` operands. Then, we have secondary data registers that can be used when all previous registers are in use, which are `r8`, `r9`, and `r10`.
    
- `Pointer Registers` - are used to store specific important address pointers. The main pointer registers are the Base Stack Pointer `rbp`, which points to the beginning of the Stack, the Current Stack Pointer `rsp`, which points to the current location within the Stack (top of the Stack), and the Instruction Pointer `rip`, which holds the address of the next instruction.

### Sub-Registers

Each `64-bit` register can be further divided into smaller sub-registers containing the lower bits, at one byte `8-bits`, 2 bytes `16-bits`, and 4 bytes `32-bits`. Each sub-register can be used and accessed on its own, so we don't have to consume the full 64-bits if we have a smaller amount of data.

![[Pasted image 20240106182056.png]]

Sub-registers can be accessed as:

|Size in bits|Size in bytes|Name|Example|
|---|---|---|---|
|`16-bit`|`2 bytes`|the base name|`ax`|
|`8-bit`|`1 bytes`|base name and/or ends with `l`|`al`|
|`32-bit`|`4 bytes`|base name + starts with the `e` prefix|`eax`|
|`64-bit`|`8 bytes`|base name + starts with the `r` prefix|`rax`|
For example, for the `bx` data register, the 16-bit is `bx`, so the 8-bit is `bl`, the 32-bit would be `ebx`, and the 64-bit would be `rbx`. The same goes for pointer registers. If we take the base stack pointer `bp`, its 16-bit sub-register is `bp`, so the 8-bit is `bpl`, the 32-bit is `ebp`, and the 64-bit is `rbp`.

The following are the names of the sub-registers for all of the essential registers in an x86_64 architecture:

|Description|64-bit Register|32-bit Register|16-bit Register|8-bit Register|
|---|---|---|---|---|
|**Data/Arguments Registers**|||||
|Syscall Number/Return value|`rax`|`eax`|`ax`|`al`|
|Callee Saved|`rbx`|`ebx`|`bx`|`bl`|
|1st arg - Destination operand|`rdi`|`edi`|`di`|`dil`|
|2nd arg - Source operand|`rsi`|`esi`|`si`|`sil`|
|3rd arg|`rdx`|`edx`|`dx`|`dl`|
|4th arg - Loop counter|`rcx`|`ecx`|`cx`|`cl`|
|5th arg|`r8`|`r8d`|`r8w`|`r8b`|
|6th arg|`r9`|`r9d`|`r9w`|`r9b`|
|**Pointer Registers**|||||
|Base Stack Pointer|`rbp`|`ebp`|`bp`|`bpl`|
|Current/Top Stack Pointer|`rsp`|`esp`|`sp`|`spl`|
|Instruction Pointer 'call only'|`rip`|`eip`|`ip`|`ipl`|

## Memory Addresses

x86 64-bit processors have 64-bit wide addresses that range from `0x0` to `0xffffffffffffffff`, so we expect the addresses to be in this range. However, RAM is segmented into various regions, like the Stack, the heap, and other program and kernel-specific regions. Each memory region has specific `read`, `write`, `execute` permissions that specify whether we can read from it, write to it, or call an address in it.

Whenever an instruction goes through the Instruction Cycle to be executed, the first step is to fetch the instruction from the address it's located at, as previously discussed. There are several types of address fetching (i.e., addressing modes) in the x86 architecture:

|Addressing Mode|Description|Example|
|---|---|---|
|`Immediate`|The value is given within the instruction|`add 2`|
|`Register`|The register name that holds the value is given in the instruction|`add rax`|
|`Direct`|The direct full address is given in the instruction|`call 0xffffffffaa8a25ff`|
|`Indirect`|A reference pointer is given in the instruction|`call 0x44d000` or `call [rax]`|
|`Stack`|Address is on top of the stack|`add rsp`|
In the above table, lower is slower. The less immediate the value is, the slower it is to fetch it.

### Address Endianness

An address endianness is the order of its bytes in which they are stored or retrieved from memory. 
There are two types of endianness: **`Little-Endian`** and **`Big-Endian`**.
- Little-Endian processors, the little-end byte of the address is filled/retrieved first `right-to-left`.
- Big-Endian processors, the big-end byte is filled/retrieved first `left-to-right`.

Of course, when retrieving the value back, the processor will also use little-endian retrieval, so the value retrieved would be the same as the original value.
This indicates that the order in which the bytes are stored/retrieved makes a big difference.

**Exemple :**

| **Address**       | **0** | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **Address Value**  |
| ----------------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ------------------ |
| **Little Endian** | 77    | 66    | 55    | 44    | 33    | 22    | 11    | 00    | 0x7766554433221100 |
| **Big Endian**    | 00    | 11    | 22    | 33    | 44    | 55    | 66    | 77    | 0x0011223344556677 |

Little-endian byte order -> as it is used with Intel/AMD x86 in most modern operating systems, so the shellcode is represented right-to-left.

>[!Important]
>`The important thing we need to take from this is knowing that our bytes are stored into memory from right-to-left.` So, if we were to push an address or a string with Assembly, we would have to push it in reverse. For example, if we want to store the word `Hello`, we would push its bytes in reverse: `o`, `l`, `l`, `e`, and finally `H`.

## Data Types

the x86 architecture supports many types of data sizes, which can be used with various instructions. The following are the most common data types we will be using with instructions:

|Component|Length|Example|
|---|---|---|
|`byte`|8 bits|`0xab`|
|`word`|16 bits - 2 bytes|`0xabcd`|
|`double word (dword)`|32 bits - 4 bytes|`0xabcdef12`|
|`quad word (qword)`|64 bits - 8 bytes|`0xabcdef1234567890`|
>[!Important]
>`Whenever we use a variable with a certain data type or use a data type with an instruction, both operands should be of the same size.`

For example, we can't use a variable defined as `byte` with `rax`, as `rax` has a size of 8 bytes. In this case, we would have to use `al`, which has the same size of 1 byte. The following table shows the appropriate data type for each sub-register:

|Sub-register|Data Type|
|---|---|
|`al`|`byte`|
|`ax`|`word`|
|`eax`|`dword`|
|`rax`|`qword`|



