
## Opérations

|Instruction|Description|Example|
|---|---|---|
|`add`|Add both operands|`add rax, rbx` -> `rax = 1 + 1` -> `2`|
|`sub`|Subtract Source from Destination (_i.e `rax = rax - rbx`_)|`sub rax, rbx` -> `rax = 1 - 1` -> `0`|
|`imul`|Multiply both operands|`imul rax, rbx` -> `rax = 1 * 1` -> `1`|

>[!Note]
In all of the above instructions, the result is always stored in the destination operand, while the source operand is not affected.

>[!Tip]
>Using xor on any register with itself will turn it into 0

>[!Note]
>the `cmp` only compares and does not store the result anywhere. The main advantage of `cmp` is that it does not affect the operands.

**Le premier "operand" est la destination**
par exemple `add rax, rbx` 
- rax = destination 
- rbx = source 
Donc additionne rax et rbx et stock le résultat dans rax.


