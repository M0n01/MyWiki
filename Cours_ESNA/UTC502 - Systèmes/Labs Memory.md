>Gaël le Maréchal

Questions :

**A votre avis, qu'elles sont les caractéristiques d'une pages mémoire suspecte ?
Quels seront les droits de la page ?**

Les caractéristiques d'une page mémoire supecte sont les suivantes :
* Permisions inappropriées : un +x par exemple
* Comportement inhabituel

**En regardant la liste des pages de ce processus, en voyant vous une de suspecte ?**

```bash
user@LabUTC502:~/labs/memory$ ps -u
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
user        1494  0.0  0.1   9304  5492 pts/0    Ss   06:20   0:00 bash
user        1503  0.0  0.0   8112  2088 pts/1    Ss+  06:20   0:00 bash
user        1505  0.0  0.1   8616  4612 pts/2    Ss+  06:20   0:00 bash
user        1513  0.0  0.0   8112  2004 pts/4    Ss+  06:20   0:00 bash
user        2509  0.0  0.0   9204  4024 pts/3    Ss+  08:08   0:00 bash
user        8818  0.0  0.0   8384  3816 pts/5    Ss+  15:03   0:00 /bin/bash --rcfile /home/u
user       19385  0.0  0.1   8244  5312 pts/6    Ss   22:48   0:00 bash
user       19396  0.0  0.0   2312   512 pts/0    S+   22:48   0:00 ./hideInMem
user       19397  0.0  0.0   9700  3272 pts/6    R+   22:48   0:00 ps -u
```

```bash
user@LabUTC502:~/labs/memory$ cat /proc/19396/maps
56276a46b000-56276a46c000 r--p 00000000 08:01 661565                     /home/user/labs/memory/hideInMem
56276a46c000-56276a46d000 r-xp 00001000 08:01 661565                     /home/user/labs/memory/hideInMem
56276a46d000-56276a46e000 r--p 00002000 08:01 661565                     /home/user/labs/memory/hideInMem
56276a46e000-56276a46f000 r--p 00002000 08:01 661565                     /home/user/labs/memory/hideInMem
56276a46f000-56276a470000 rw-p 00003000 08:01 661565                     /home/user/labs/memory/hideInMem
56276b5dd000-56276b5de000 rw-p 00000000 00:00 0                          [heap]
56276b5de000-56276b5df000 rwxp 00000000 00:00 0                          [heap]
56276b5df000-56276b600000 rw-p 00000000 00:00 0                          [heap]
7f11a4e0b000-7f11a4e30000 r--p 00000000 08:01 262170                     /usr/lib/x86_64-linux-gnu/libc-2.31.so
7f11a4e30000-7f11a4f7b000 r-xp 00025000 08:01 262170                     /usr/lib/x86_64-linux-gnu/libc-2.31.so
7f11a4f7b000-7f11a4fc5000 r--p 00170000 08:01 262170                     /usr/lib/x86_64-linux-gnu/libc-2.31.so
7f11a4fc5000-7f11a4fc6000 ---p 001ba000 08:01 262170                     /usr/lib/x86_64-linux-gnu/libc-2.31.so
7f11a4fc6000-7f11a4fc9000 r--p 001ba000 08:01 262170                     /usr/lib/x86_64-linux-gnu/libc-2.31.so
7f11a4fc9000-7f11a4fcc000 rw-p 001bd000 08:01 262170                     /usr/lib/x86_64-linux-gnu/libc-2.31.so
7f11a4fcc000-7f11a4fd2000 rw-p 00000000 00:00 0 
7f11a4fe5000-7f11a4fe6000 r--p 00000000 08:01 262165                     /usr/lib/x86_64-linux-gnu/ld-2.31.so
7f11a4fe6000-7f11a5006000 r-xp 00001000 08:01 262165                     /usr/lib/x86_64-linux-gnu/ld-2.31.so
7f11a5006000-7f11a500e000 r--p 00021000 08:01 262165                     /usr/lib/x86_64-linux-gnu/ld-2.31.so
7f11a500f000-7f11a5010000 r--p 00029000 08:01 262165                     /usr/lib/x86_64-linux-gnu/ld-2.31.so
7f11a5010000-7f11a5011000 rw-p 0002a000 08:01 262165                     /usr/lib/x86_64-linux-gnu/ld-2.31.so
7f11a5011000-7f11a5012000 rw-p 00000000 00:00 0 
7ffc29539000-7ffc2955a000 rw-p 00000000 00:00 0                          [stack]
7ffc295ad000-7ffc295b1000 r--p 00000000 00:00 0                          [vvar]
7ffc295b1000-7ffc295b3000 r-xp 00000000 00:00 0                          [vdso]
```

Cette ligne est suspecte :
```
56276b5de000-56276b5df000 rwxp 00000000 00:00 0                          [heap]
```
Les droits `rwxp`.

**Quel message contient cette page ?**

```bash
user@LabUTC502:~/labs/memory$ gcore 19396
0x00007f11a4ef9e8e in read () from /lib/x86_64-linux-gnu/libc.so.6
Saved corefile core.19396
[Inferior 1 (process 19396) detached]
```

```bash
             uu$:$:$:$:$:$uu
          uu$$$$$$$$$$$$$$$$$uu
         u$$$$$$$$$$$$$$$$$$$$$u
         u$$$$$$$$$$$$$$$$$$$$$$$u
       u$$$$$$$$$$$$$$$$$$$$$$$$$u
       u$$$$$$$$$$$$$$$$$$$$$$$$$u
       u$$$$$$*   *$$$*   *$$$$$$u
       *$$$$*      u$u       $$$$*
        $$$u       u$u       u$$$
        $$$u      u$$$u      u$$$
         *$$$$uu$$$   $$$uu$$$$*
          *$$$$$$$*   *$$$$$$$*
            u$$$$$$$u$$$$$$$u
             u$*$*$*$*$*$*$u
  uuu        $$u$ $ $ $ $u$$       uuu
 u$$$$        $$u$u$u$u$u$$       u$$$$
  $$$$$uu      *$$$$$$$$$*     uu$$$$$$
u$$$$$$$$$$$      *****    uuuu$$$$$$$$$
$$$$***$$$$$$$$$$uuu   uu$$$$$$$$$***$$$*
 ***      **$$$$$$$$$$$uu **$***
          uuuu **$$$$$$$$$$uuu
 u$$$uuu$$$$$$$$$uu **$$$$$$$$$$$uuu$$$
 $$$$$$$$$$****           **$$$$$$$$$$$*
   *$$$$$*                      **$$$$**
     $$$*                         $$$$*
The secret keypass is : Catched!

```
