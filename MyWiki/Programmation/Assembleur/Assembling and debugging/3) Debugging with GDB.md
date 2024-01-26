#gdb #debugging #assembly #assembleur 

|Step|Description|
|---|---|
|`Break`|Setting breakpoints at various points of interest|
|`Examine`|Running the program and examining the state of the program at these points|
|`Step`|Moving through the program to examine how it acts with each instruction and with user input|
|`Modify`|Modify values in specific registers or addresses at specific breakpoints, to study how it would affect the execution|

## Break

The first step of debugging is setting `breakpoints` to stop the execution at a specific location or when a particular condition is met.

`Breakpoints` also allow us to stop the program's execution at that point so that we can step into each instruction and examine how it changes the program and values.

We can set a breakpoint at a specific address or for a particular function. To set a breakpoint, we can use the `break` or `b` command along with the address or function name we want to break at.

For example, to follow all instructions run by our program, let's break at the `_start` function :
```bash
gef➤  b _start
```

Now, in order to start our program, we can use the `run` or `r` command :
```bash
gef➤  r
```

If we want to set a breakpoint at a certain address, like `_start+10`, we can either `b *_start+10` or `b *0x40100a`:
```shell-session
gef➤  b *0x40100a
ou
gef➤  b *_start+10
```

>[!Note]
>Once the program is running, if we set another breakpoint, like `b *0x401005`, in order to continue to that breakpoint, we should use the `continue` or `c` command. If we use `run` or `r` again, it will run the program from the start.

`info breakpoint` -> to see what breakpoints we have at any point of the execution.

We can also `disable`, `enable`, or `delete` any breakpoint. Furthermore, GDB also supports setting conditional breaks that stop the execution when a specific condition is met.

## Examine

The next step of debugging is `examining` the values in registers and addresses.

`GEF` plugin automates many steps that we usually take at every breakpoint, like examining the registers, the stack, and the current assembly instructions.

The `ADDRESS` is the address or register we want to examine, while `FMT` is the examine format. The examine format `FMT` can have three parts:

|Argument|Description|Example|
|---|---|---|
|`Count`|The number of times we want to repeat the examine|`2`, `3`, `10`|
|`Format`|The format we want the result to be represented in|`x(hex)`, `s(string)`, `i(instruction)`|
|`Size`|The size of memory we want to examine|`b(byte)`, `h(halfword)`, `w(word)`, `g(giant, 8 bytes)`|
#### Instructions

For example, if we wanted to examine the next four instructions in line, we will have to examine the `$rip` register (which holds the address of the next instruction), and use `4` for the `count`, `i` for the `format`, and `g` for the `size` (for 8-bytes or 64-bits). So, the final examine command would be `x/4ig $rip`, as follows:
```shell-session
gef➤  x/4ig $rip
```

This can help us as we go through a program in examining certain areas and what instructions they may contain.

#### Strings

We can also examine a variable stored at a specific memory address. We know that our `message` variable is stored at the `.data` section on address `0x402000` from our previous disassembly. We also see the upcoming command `movabs rsi, 0x402000`, so we may want to examine what is being moved from `0x402000`.

In this case, we will not put anything for the `Count`, as we only want one address (1 is the default), and will use `s` as the format to get it in a string format rather than in hex:
```shell-session
gef➤  x/s 0x402000
```
As we can see, we can see the string at this address represented as text rather than hex characters.

>[!Note]
>If we don't specify the `Size` or `Format`, it will default to the last one we used.

#### Addresses

The most common format of examining is hex `x`.
We often need to examine addresses and registers containing hex data, such as memory addresses, instructions, or binary data. Let us examine the same previous instruction, but in `hex` format, to see how it looks:
```shell-session
x/wx 0x401000
```

We see instead of `mov eax,0x1`, we get `0x000001b8`, which is the hex representation of the `mov eax,0x1` machine code in little-endian formatting.

- This is read as: `b8 01 00 00`.

## Step

The third step of debugging is `stepping` through the program one instruction or line of code at a time.

>[!Note]
>The instruction shown with the `->` symbol is where we are at, and it has not yet been processed.

To move through the program, there are three different commands we can use: `stepi` and `step`.

#### Step Instruction

The `stepi` or `si` command will step through the assembly instructions one by one, which is the smallest level of steps possible while debugging.

#### Step Count

Similarly to examine, we can repeat the `si` command by adding a number after it.
```shell-session
gef➤  si 3
```

>[!Tip]
>You can hit the `return`/`enter` empty in order to repeat the last command.

#### Step

The `step` or `s` command, on the other hand, will continue until the following line of code is reached or until it exits from the current function. If we run an assembly code, it will break when we exit the current function `_start`.

If there's a call to another function within this function, it'll break at the beginning of that function. Otherwise, it'll break after we exit this function after the program's end.
```shell-session
gef➤  step
```
We see that the execution continued until we reached the exit from the `_start` function, so we reached the end of the program and `exited normally` without any errors.

>[!Note]
>There's also the `next` or `n` command, which will also continue until the next line, but will skip any functions called in the same line of code, instead of breaking at them like `step`. There's also the `nexti` or `ni`, which is similar to `si`, but skips functions calls

## Modify

The final step of debugging is `modifying` values in registers and addresses at a certain point of execution. This helps us in seeing how this would affect the execution of the program.

#### Addresses

To modify values in GDB, we can use the `set` command. However, we will utilize the `patch` command in `GEF` to make this step much easier.

As we can see, we have to provide the `type/size` of the new value, the `location` to be stored, and the `value` we want to use. So, let's try changing the string stored in the `.data` section (at address `0x402000` as we saw earlier) to the string `Patched!\n`.

We will break at the first `syscall` at `0x401019`, and then do the patch, as follows:

```shell-session
gef➤  break *0x401019

Breakpoint 1 at 0x401019
gef➤  r
gef➤  patch string 0x402000 "Patched!\\x0a"
gef➤  c

Continuing.
Patched!
 Academy!
```

#### Registers

We also note that we did not replace the entire string. This is because we only modified the characters up to the length of our string and left the remainder of the old string. Finally, the `printf` function specified a length of `0x12` of bytes to be printed.

To fix this, let's modify the value stored in `$rdx` to the length of our string, which is `0x9`. We will only patch a size of one byte.
Let us demonstrate using `set` to modify `$rdx`, as follows:
```shell-session
gef➤  break *0x401019

Breakpoint 1 at 0x401019
gef➤  r
gef➤  patch string 0x402000 "Patched!\\x0a"
gef➤  set $rdx=0x9
gef➤  c

Continuing.
Patched!
```
We see that we successfully modified the final printed string and have the program output something of our choosing. The ability to modify values of registers and addresses will help us a lot through debugging and binary exploitation, as it allows us to test various values and conditions without having to change the code and recompile the binary every time.

>[!Important]
>`The ability to set breakpoints to stop the execution, step through a program and each of its instructions, examine various data and addresses at each point, and modify values when needed, enables us to do proper debugging and reverse engineering`.


## Exo

Download the attached file, and find the hex value in 'rax' when we reach the instruction at <_start+16>?
```bash
gef➤  disassemble _start
gef➤  b _start 
gef➤  run
gef➤  si 1
gef➤  si 1
gef➤  p/x $rax
```












