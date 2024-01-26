#assemble #disassemble #assembly 

Upon assembling our code with the tool `nasm`, it understands the various parts of the file and then correctly assembles them to be properly run during run time.

After we assemble our code with `nasm`, we can link it using `ld` to utilize various OS features and libraries.

## Assembling

First, we will copy the above code into a file called `helloWorld.s`.

>[!Note]
>Assembly files usually use the `.s` or the `.asm` extensions.

Assemble the file using `nasm` :
```bash
nasm -f elf64 helloWorld.s
```

>[!Note]
>The `-f elf64` flag is used to note that we want to assemble a 64-bit assembly code. If we wanted to assemble a 32-bit code, we would use `-f elf`.

This should output a `helloWorld.o` object file, which is then assembled into machine code, along with the details of all variables and sections. This file is not executable just yet.

## Linking

The final step is to link our file using `ld`. The `helloWorld.o` object file, though assembled, still cannot be executed. This is because many references and labels used by `nasm` need to be resolved into actual addresses, along with linking the file with various OS libraries that may be needed.

This is why a Linux binary is called `ELF`, which stands for an `Executable and Linkable Format`. 

Link a file using `ld` :
```bash
ld -o helloWorld helloWorld.o
```
Once we link the file with `ld`, we should have the final executable file.

## Disassembling

To disassemble a file -> the tool **`objdump`**. 
Which dumps machine code from a file and interprets the assembly instruction of each hex code. We can disassemble a binary using the `-D` flag.

>[!Note]
>We will also use the flag `-M intel`, so that `objdump` would write the instructions in the Intel syntax

Disassemble a file (executable) :
```bash
objdump -M intel -d helloWorld
```

If we wanted to only show the assembly code, without machine code or addresses, we could add the `--no-show-raw-insn --no-addresses` flags :
```bash
objdump -M intel --no-show-raw-insn --no-addresses -d helloWorld
```

The `-d` flag will only disassemble the `.text` section of our code. To dump any strings, we can use the `-s` flag, and add `-j .data` to only examine the `.data` section. This means that we also do not need to add `-M intel`.
```bash
objdump -sj .data helloWorld
```
