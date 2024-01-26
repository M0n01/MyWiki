#gdb #debugger #debug 

Debugging is a term used for finding and removing issues (i.e., bugs) from our code, hence the name de-bugging.

It is not efficient to keep changing our code until it does what we expect of it. Instead, we perform debugging by setting breakpoints and seeing how our program acts on each of them and how our input changes between them, which should give us a clear idea of what is causing the `bug`.

There are other similar debuggers for Linux, like [Radare](https://www.radare.org/r/) and [Hopper](https://www.hopperapp.com), and for Windows, like [Immunity Debugger](https://www.immunityinc.com/products/debugger/) and [WinGDB]. There are also powerful debuggers available for many platforms, like [IDA Pro](https://www.hex-rays.com/products/ida/) and [EDB](https://github.com/eteran/edb-debugger).

## GDB

One of the great features of `GDB` is its support for third-party plugins. An excellent plugin that is well maintained and has good documentation is [GEF](https://github.com/hugsy/gef). GEF is a free and open-source GDB plugin that is built precisely for reverse engineering and binary exploitation.

To add GEF to GDB :
```bash
wget -O ~/.gdbinit-gef.py -q https://gef.blah.cat/py
echo source ~/.gdbinit-gef.py >> ~/.gdbinit
```
we can run gdb to debug our `HelloWorld` binary using the following commands, and GEF will be loaded automatically.
## Getting Started

We can now run gdb to debug our `HelloWorld` binary : 
```bash
gdb -q ./helloWorld
```

Once GDB is started, we can use the info command to view general information about the program, like its functions or variables.

>[!Tip]
>If we want to understand how any command runs within GDB, we can use the help CMD command to get its documentation. For example, we can try executing help info

#### Functions
The `info` command to check which `functions` are defined :
```shell-session
gef➤  info functions
```

#### Variables
The `info variables` command to view all available variables :
```shell-session
gef➤  info variables
```
We can do many things with functions, but we will focus on two main points: Disassembly and Breakpoints.

## Disassemble

To view the instructions within a specific function, we can use the `disassemble` or `disas` command along with the function name :
```shell-session
gef➤  disas _start
```

We need to focus on the main thing from this disassembly: the memory addresses for each instruction and operands (i.e., arguments).

==`Having the memory address is critical for examining the variables/operands and setting breakpoints for a certain instruction.`==







