#code #assembleur #assembly #syntaxe

An Assembly code is line-based, which means that the file is processed line-by-line, executing the instruction of each line.

Code pour afficher "Hello HTB Academy !" : 
![[Pasted image 20240107152956.png]]

| Section         | Description                                                                                         |
| --------------- | --------------------------------------------------------------------------------------------------- |
| `global _start` | This is a `directive` that directs the code to start executing at the `_start` label defined below. |
| `section .data` | This is the `data` section, which should contain all of the variables.                              |
| `section .text` | This is the `text` section containing all of the code to be executed.                               |

## Directives

`global _start` : instructs the machine to start processing the instructions after the `_start` label. So, the machine goes to the `_start` label and starts executing the instructions there, which will print the message on the screen.

## Variables

**The `data` section :**
holds our variables to make it easier for us to define variables and reuse them without writing them multiple times. Once we run our program, all of our variables will be loaded into memory in the `data` segment.

When we run the program, it will load any variables we have defined into memory so that they will be ready for usage when we call them.

We can define variables using `db` for a list of bytes, `dw` for a list of words, `dd` for a list of digits, and so on. We can also label any of our variables so we can call it or reference it later. The following are some examples of defining variables:

|Instruction|Description|
|---|---|
|`db 0x0a`|Defines the byte `0x0a`, which is a new line.|
|`message db 0x41, 0x42, 0x43, 0x0a`|Defines the label `message => abc\n`.|
|`message db "Hello World!", 0x0a`|Defines the label `message => Hello World!\n`.|

We can use the `equ` instruction with the `$` token to evaluate an expression, like the length of a defined variable's string. However, the labels defined with the `equ` instruction are constants, and they cannot be changed later.

For example, the following code defines a variable and then defines a constant for its length:

```nasm
section .data
    message db "Hello World!", 0x0a
    length  equ $-message
```

>[!note]
>the `$` token indicates the current distance from the beginning of the current section. As the `message` variable is at the beginning of the `data` section, the current location, i.e,. value of `$`, equals the length of the string. 

## Code

**The `.text` section :**
This section holds all of the assembly instructions and loads them to the `text` memory segment. Once all instructions are loaded into the `text` segment, the processor starts executing them one after another.

The default convention is to have the `_start` label at the beginning of the `.text` section, which -as per the `global _start` directive- starts the main code that will be executed as the program runs.

We can define other labels within the `.text` section, for loops and other functions.

The `text` segment within the memory is read-only, so we cannot write any variables within it. The `data` section, on the other hand, is read/write, which is why we write our variables to it. However, the `data` segment within the memory is not executable, so any code we write to it cannot be executed. This separation is part of memory protections to mitigate things like buffer overflows and other types of binary exploitation.

>[!Tip]
>We can add comments to our assembly code with a semi-colon `;`.


