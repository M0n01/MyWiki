>Gaël le Maréchal

### Manipulation 1
Quels sont les différents FS montés, à quels fichiers spéciaux sont-ils associés, quels sont leurs répertoires de montage ?
```bash
user@LabUTC502:~/cnam-utc502-os$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=1989676k,nr_inodes=497419,mode=755)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,noexec,relatime,size=402596k,mode=755)
/dev/sda1 on / type ext4 (rw,relatime,errors=remount-ro)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev)
tmpfs on /run/lock type tmpfs (rw,nosuid,nodev,noexec,relatime,size=5120k)
cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot)
pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
none on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime,mode=700)
systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=29,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=10882)
mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime)
debugfs on /sys/kernel/debug type debugfs (rw,nosuid,nodev,noexec,relatime)
tracefs on /sys/kernel/tracing type tracefs (rw,nosuid,nodev,noexec,relatime)
hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,pagesize=2M)
fusectl on /sys/fs/fuse/connections type fusectl (rw,nosuid,nodev,noexec,relatime)
configfs on /sys/kernel/config type configfs (rw,nosuid,nodev,noexec,relatime)
/var/lib/snapd/snaps/core18_2538.snap on /snap/core18/2538 type squashfs (ro,nodev,relatime,x-gdu.hide)
tmpfs on /run/user/1000 type tmpfs (rw,nosuid,nodev,relatime,size=402592k,nr_inodes=100648,mode=700,uid=1000,gid=1000)
binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,nosuid,nodev,noexec,relatime)
tracefs on /sys/kernel/debug/tracing type tracefs (rw,nosuid,nodev,noexec,relatime)
/var/lib/snapd/snaps/core_16202.snap on /snap/core/16202 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/core20_2105.snap on /snap/core20/2105 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/btop_655.snap on /snap/btop/655 type squashfs (ro,nodev,relatime,x-gdu.hide)
```

### Manipulation 2
Quelle est la place libre, en blocs de 512 octets, restant sur chaque FS ? Quel est le pourcentage reservé à root pour chaque FS ?
```bash
user@LabUTC502:~/cnam-utc502-os$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           394M  972K  393M   1% /run
/dev/sda1       100G   11G   84G  12% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
/dev/loop0       56M   56M     0 100% /snap/core18/2538
tmpfs           394M   64K  394M   1% /run/user/1000
/dev/loop2      106M  106M     0 100% /snap/core/16202
/dev/loop1       64M   64M     0 100% /snap/core20/2105
/dev/loop3      2.0M  2.0M     0 100% /snap/btop/655
```
Pour connaître le pourcentage réservé à root
```bash
user@LabUTC502:~/cnam-utc502-os$ echo "Pourcentage réservé à root : $(echo "scale=2; ($(sudo dumpe2fs -h /dev/sda1 | grep "Reserved block count" | awk '{print $NF}') / $(sudo dumpe2fs -h /dev/sda1 | grep "Block count" | awk '{print $NF}')) * 100" | bc)%"
[sudo] password for user: 
dumpe2fs 1.46.2 (28-Feb-2021)
dumpe2fs 1.46.2 (28-Feb-2021)
Pourcentage réservé à root : 4.00%
```

### Manipulation 3
Quelles sont les tailles des blocs dans les FS ext2 montés sur votre système ?
```bash
user@LabUTC502:~/cnam-utc502-os$ sudo tune2fs -l /dev/sda1 | grep "Block size"
Block size:               4096
```

### Manipulation 4
Prenez une image iso CD-ROM ayant un format compatible ISO9660 exemple un live cd d’une distribution linux. Montez-la, listez son arborescence et démontez-le (éjectez-le par la commande eject). Vous pouvez également faire la manip avec un disque ou une clé USB. L’idée est bien de monter ce périphérique à la main et pas de laisser faire l’automount du système. Le FS importe peu du moment que vous monterez manuellement le volume.

Pour cela j'utiliserai la distribution DSL (Damn Small Linux).
Créer un répertoire où je monterai l'iso
```bash
sudo mkdir /mnt/iso
```
Monter l'iso
```bash
sudo mount -o loop dsl-4.11.rc1.iso /mnt/iso
```
Lister l'arborescence
```bash
ls -l /mnt/iso
user@LabUTC502:~/cnam-utc502-os$ ls -l /mnt/iso
total 5
dr-xr-xr-x 3 root root 2048 Jul 25  2004 boot
-r--r--r-- 1 root root  391 May  3  2004 index.html
dr-xr-xr-x 2 root root 2048 Jul 31  2012 KNOPPIX
```
Démonter l'iso
```bash
sudo umount /mnt/iso
```

#### Manipulation 5
Dans une autre console (Ctrl+Alt+F2), loggez-vous sous votre nom d’utilisateur et allez dans un répertoire /usr/local et visualisez un fichier de votre choix dans ce répertoire avec la commande vi. Revenez à votre console root et essayer de démonter /usr/local. Que se passe-t-il ? Que faire pour arriver à démonter ?

On ne peut pas démonter `/usr/local` car il est "occupé". En effet un processus est en cours d’exécution dans celui-ci. Pour le démonter, il faut que le processus en question soit stoppé.

### Manipulation 6
Reprenez votre clé USB où votre support. Vous devez maintenant le monter et faire en sorte qu’il ne soit accessible qu’à un seul utilisateur (par exemple vous !). Aidez-vous du man de mount.

Monter l'image iso :
```bash
user@LabUTC502:~/cnam-utc502-os$ sudo mount -o loop,uid=user dsl-4.11.rc1.iso /mnt/iso/

mount: /mnt/iso: WARNING: source write-protected, mounted read-only.
```
- `loop`: Permet de monter des fichiers images comme des disques.
- `uid`: Spécifie l'identifiant d'utilisateur (UID) du propriétaire du point de montage.

Vérifier le montage :
```bash
user@LabUTC502:~/cnam-utc502-os$ mount | grep /mnt/iso

/home/user/cnam-utc502-os/dsl-4.11.rc1.iso on /mnt/iso type iso9660 (ro,relatime,nojoliet,check=s,map=n,blocksize=2048,uid=1000,iocharset=utf8)
```
On peut voir que le montage s'est correctement effectué.

Vérifier que c'est le bon UID : 
```bash
user@LabUTC502:~/cnam-utc502-os$ cat /etc/passwd | grep user

user:x:1000:1000:user,,,:/home/user:/bin/bash
suser:x:117:65534::/home/suser:/usr/sbin/nologin
userquota:x:1002:1002:,,,:/home/userquota:/bin/bash
```

### Manipulation 7
Reprenez la Manipulation 4 en considérant que vous êtes dans un environnement de base UNIX sans la couche gnome, que faut il faire pour que le montage du support se fasse à la volée ?

Il faut modifier le fichier `/etc/fstab`.
Tout d'abord on doit connaître l'UUID correspondante :
```bash
user@LabUTC502:~/cnam-utc502-os$ sudo blkid

/dev/sda1: UUID="14b32574-5ad2-4e84-b7c4-7c3396b4beed" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="45bc040b-01"
/dev/sda5: UUID="e24bf2c8-bb75-4fd7-95da-45168f7ec5dc" TYPE="swap" PARTUUID="45bc040b-05"
/dev/loop0: TYPE="squashfs"
/dev/loop1: TYPE="squashfs"
/dev/loop2: TYPE="squashfs"
/dev/loop3: TYPE="squashfs"
/dev/loop4: BLOCK_SIZE="2048" UUID="2012-08-03-18-12-29-00" LABEL="KNOPPIX" TYPE="iso9660"
```

Ajouter dans `/etc/fstab` : 
```bash
UUID=2012-08-03-18-12-29-00 /mnt/iso auto noauto,user 0 0
```
- **`auto`** : Le type de système de fichiers sera détecté automatiquement.
- **`noauto`** : Le système ne montera pas automatiquement le périphérique au démarrage.
- **`user`** : Permet aux utilisateurs ordinaires de monter le périphérique.
### Manipulation 8
Laissez vous aller votre imagination et testez les différentes options de mount.
```bash
user@LabUTC502:~/cnam-utc502-os$ man mount
```

### Manipulation 9 - Utilisation de LVM
Attention : certaines manipulations détruisent la table des partitions. Prenez les précautions nécessaires à la sauvegarde de vos données (au moins cette table pour la restaurer). Je vous conseille de faire un snapshot de votre VM avant ces manipulations.

Installer lvm2 : 
```bash
sudo apt-get install lvm2
```

Etapes :
1. Sur l'espace libre de votre disque dur, créez deux partitions de tailles différentes, l'une de 396Mo, l'autre de 992Mo.
```bash
user@LabUTC502:~/cnam-utc502-os$ sudo parted /dev/sda
GNU Parted 3.4
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) mkpart primary 0% 396MiB                                         
Warning: You requested a partition from 0.00B to 415MB (sectors 0..811007).
The closest location we can manage is 512B to 1048kB (sectors 1..2047).
Is this still acceptable to you?
Yes/No? Yes                                                               
Warning: The resulting partition is not properly aligned for best performance: 1s % 2048s !=
0s
Ignore/Cancel? I                                                          
(parted) mkpart primary 396MiB 1388MiB                                  
Warning: You requested a partition from 415MB to 1455MB (sectors 811008..2842623).
The closest location we can manage is 109GB to 109GB (sectors 212539392..212539392).
Is this still acceptable to you?
Yes/No? yes                                                               
```

### Manipulation 10 : copie d’un CD avec dd et montage en loopback
Vous utiliserez la commande dd pour créer une image d’un CD. La destination doit être un fichier que vous nommerez, par exemple, « image_cd.iso ». Quelle est la valeur à mettre dans l’option bs (block size) de dd ?
```bash
user@LabUTC502:~/cnam-utc502-os$ lsblk
NAME   MAJ:MIN RM    SIZE RO TYPE MOUNTPOINT
loop0    7:0    0   55.6M  1 loop /snap/core18/2538
loop1    7:1    0   63.9M  1 loop /snap/core20/2105
loop2    7:2    0  105.8M  1 loop /snap/core/16202
loop3    7:3    0    1.9M  1 loop /snap/btop/655
loop4    7:4    0   50.6M  0 loop /mnt/iso
sda      8:0    0  102.3G  0 disk 
├─sda1   8:1    0  101.3G  0 part /
├─sda2   8:2    0      1K  0 part 
├─sda3   8:3    0 1023.5K  0 part 
├─sda4   8:4    0    512B  0 part 
└─sda5   8:5    0    976M  0 part [SWAP]
sr0     11:0    1   1024M  0 rom  

user@LabUTC502:~/cnam-utc502-os$ sudo dd if=/dev/sda4 of=imagecd.iso bs=2048
[sudo] password for user: 
0+1 records in
0+1 records out
512 bytes copied, 0.00117082 s, 437 kB/s

user@LabUTC502:~/cnam-utc502-os$ ls -lh imagecd.iso 
-rw-r--r-- 1 root root 512 Dec 22 21:23 imagecd.iso
```

Vous éjecterez ensuite le CD. Effectuez le montage de l’image en mode « loopback » sur le point de montage de votre choix (par exemple, celui du CD). Expliquez votre démarche.
```bash
user@LabUTC502:~/cnam-utc502-os$ eject /dev/sda4
user@LabUTC502:~/cnam-utc502-os$ sudo mkdir /mnt/cd_image
user@LabUTC502:~/cnam-utc502-os$ sudo mount -o loop imagecd.iso /mnt/cd_image/
```
On effectue le montage tel que dans la manipulation 4.

### Manipulation 11 : Sauvegarde d’une arborescence avec tar et cpio, essai avec pax (commande de l'ISO)
Vous choisirez une arborescence à sauvegarder.
Vous effectuerez et expliquerez les manipulations suivantes avec tar, cpio puis pax (qui unifie les deux commandes) :
- archivage
```bash
user@LabUTC502:~/cnam-utc502-os$ tar -cvf sauvegarde.tar sauvegarde/
sauvegarde/
sauvegarde/fichierTest

user@LabUTC502:~/cnam-utc502-os$ ls -lah
total 51M
drwxr-xr-x  8 user user      4.0K Dec 22 21:33 .
drwxr-xr-x 22 user bluetooth 4.0K Dec 21 12:07 ..
drwxr-xr-x  4 user user      4.0K Aug  1  2022 content
drwxr-xr-x  8 user user      4.0K Aug  9  2022 cours
-rw-r--r--  1 user user       51M Dec 21 13:50 dsl-4.11.rc1.iso
drwxr-xr-x  8 user user      4.0K Dec 21 11:35 .git
drwxr-xr-x  3 user user      4.0K Dec 21 13:22 .idea
-rw-r--r--  1 root root       512 Dec 22 21:23 imagecd.iso
-rw-r--r--  1 user user      1.2K Aug  4  2022 makefile
drwxr-xr-x  2 user user      4.0K Dec 22 21:33 sauvegarde
-rw-r--r--  1 user user       10K Dec 22 21:33 sauvegarde.tar
drwxr-xr-x  4 user user      4.0K Aug  4  2022 venv
```
Avec **`cpio`** :
```bash
find sauvegarde/ | cpio -o > sauvegarde.cpio
```
Avec **`pax`** :
```bash
pax -w -f sauvegarde.pax sauvegarde/
```

- archivage avec compression (notez la différence de taille) lecture du contenu de l’archive
```bash
user@LabUTC502:~/cnam-utc502-os$ tar -cvzf sauvegarde.tar.gz sauvegarde
sauvegarde/
sauvegarde/fichierTest

user@LabUTC502:~/cnam-utc502-os$ ls -lah
total 51M
drwxr-xr-x  8 user user      4.0K Dec 22 21:35 .
drwxr-xr-x 22 user bluetooth 4.0K Dec 21 12:07 ..
drwxr-xr-x  4 user user      4.0K Aug  1  2022 content
drwxr-xr-x  8 user user      4.0K Aug  9  2022 cours
-rw-r--r--  1 user user       51M Dec 21 13:50 dsl-4.11.rc1.iso
drwxr-xr-x  8 user user      4.0K Dec 21 11:35 .git
drwxr-xr-x  3 user user      4.0K Dec 21 13:22 .idea
-rw-r--r--  1 root root       512 Dec 22 21:23 imagecd.iso
-rw-r--r--  1 user user      1.2K Aug  4  2022 makefile
drwxr-xr-x  2 user user      4.0K Dec 22 21:33 sauvegarde
-rw-r--r--  1 user user       10K Dec 22 21:33 sauvegarde.tar
-rw-r--r--  1 user user       183 Dec 22 21:35 sauvegarde.tar.gz
drwxr-xr-x  4 user user      4.0K Aug  4  2022 venv
```
10Ko -> sauvegarde.tar
183o -> sauvegarde.tar.gz
Avec **`cpio`** :
```bash
find sauvegarde/ | cpio -o | gzip > sauvegarde
```
Avec **`pax`** :
```bash
pax -w -z -f sauvegarde.pax.gz sauvegarde/
```

- restauration de l’intégralité de l’archive
```bash
user@LabUTC502:~/cnam-utc502-os$ tar -tvf sauvegarde.tar
drwxr-xr-x user/user         0 2023-12-22 21:33 sauvegarde/
-rw-r--r-- user/user        38 2023-12-21 16:07 sauvegarde/fichierTest
```
Avec **`cpio`** :
```bash
mkdir sauvegarde_res
cpio -id < archive.cpio -D sauvegarde_res
```
Avec **`pax`** :
```bash
mkdir sauvegarde_repertoire
pax -r -f sauvegarde.pax -s ',^,sauvegarde_repertoire/,'
```

- restauration d’un fichier choix dans l’archive à un endroit déterminé
```bash
user@LabUTC502:~/cnam-utc502-os$ mkdir sauvegarde_res
user@LabUTC502:~/cnam-utc502-os$ tar -xvf sauvegarde.tar -C sauvegarde_res
sauvegarde/
sauvegarde/test.txt
user@LabUTC502:~/cnam-utc502-os$ ls -lah
total 51M
drwxr-xr-x  9 user user      4.0K Dec 22 21:54 .
drwxr-xr-x 22 user bluetooth 4.0K Dec 21 12:07 ..
drwxr-xr-x  4 user user      4.0K Aug  1  2022 content
drwxr-xr-x  8 user user      4.0K Aug  9  2022 cours
-rw-r--r--  1 user user       51M Dec 21 13:50 dsl-4.11.rc1.iso
drwxr-xr-x  8 user user      4.0K Dec 21 11:35 .git
drwxr-xr-x  3 user user      4.0K Dec 21 13:22 .idea
-rw-r--r--  1 root root       512 Dec 22 21:23 imagecd.iso
-rw-r--r--  1 user user      1.2K Aug  4  2022 makefile
drwxr-xr-x  2 user user      4.0K Dec 22 21:33 sauvegarde
drwxr-xr-x  2 user user      4.0K Dec 22 21:54 sauvegarde_res
-rw-r--r--  1 user user       10K Dec 22 21:33 sauvegarde.tar
-rw-r--r--  1 user user       183 Dec 22 21:35 sauvegarde.tar.gz
drwxr-xr-x  4 user user      4.0K Aug  4  2022 venv
```
Avec **`cpio`** :
```bash
cpio -ivd < archive.cpio sauvegarde.pax -D sauvegarde_res
```

### Manipulation 12 : sauvegarde incrémentale d’une arborescence avec dump et restore
Vous choisirez d’effectuer cette manipulation avec l’une des commandes de sauvegarde (tar, cpio, pax).
1. Commencez par faire une sauvegarde intégrale.
```bash
tar -cvf sauvegarde_integrale.tar sauvegarde
```

2. Faites quelques modifications, créations et suppressions de fichiers.
```bash
rm sauvegarde/modif.txt
```

3. Faites une sauvegarde incrémentale et vérifiez que le contenu de la sauvegarde est conforme à vos attentes.
```bash
tar -cvf sauvegarde.tar --listed-incremental=sauvegarde.snar sauvegarde
tar: sauvegarde: Directory is new
tar: sauvegarde/hello: Directory is new
sauvegarde/
sauvegarde/coucou/
sauvegarde/modif.txt
sauvegarde/suppr.txt
```

5. Faites une restauration intégrale de l’archive dans un répertoire différent de celui d’origine et vérifiez que tout a été restauré. Notez également les changements à chaque étape..
```bash
tar -cvf sauvegarde.tar --listed-incremental=sauvegarde.snar sauvegarde
sauvegarde/ 
sauvegarde/coucou/

tar -xvf sauvegarde.tar -C restauration_sauvegarde 
sauvegarde/ 
sauvegarde/coucou/
```

### Manipulation 13 : Intégrité d’un système de fichiers

```bash
# Vérification de l'intégrité 
user@LabUTC502:~$ sudo umount /dev/sda2 
user@LabUTC502:~$ sudo e2fsck /dev/sda2 
e2fsck 1.45.5 (07-Jan-2020) 
/dev/sda2: clean, n/n files, m/m blocks 
user@LabUTC502:~$ sudo mount /dev/sda2 / 
#
user@LabUTC502:~$ sudo debugfs -R stats /dev/sda1 
user@LabUTC502:~$ sudo fsck -b 0xfcde10a7 /dev/sda1 
user@LabUTC502:~$ sudo tune2fs -c 30 /dev/sda1 
tune2fs 1.46.2 (28-Feb-2021) 
Setting maximal mount count to 30
```

### Manipulation 14 : Debug d’un système de fichiers

Repérez le numéro d’inode d’un fichier de votre choix (auquel vous ne tenez pas particulièrement² ?) avec la commande ls et l’option appropriée. Ouvrez un FS en écriture. Essayez de modifier des blocs particuliers concernant le fichier que vous avez choisi. Ensuite lancez une vérification du système de fichiers.

1. - Repérer le numéro d'inode d'un fichier, ici 2108328
```bash
user@LabUTC502:~/cnam-utc502-os$ ls -i dsl-4.11.rc1.iso 
2108328 dsl-4.11.rc1.iso
```

- Ouvrir le système de fichiers en écriture avec debugfs
```bash
user@LabUTC502:~/cnam-utc502-os$ sudo debugfs /dev/sda1
[sudo] password for user: 
debugfs 1.46.2 (28-Feb-2021)
debugfs:  clri 2108328
clri: Filesystem opened read/only
debugfs:  
```

- Activer le mode écriture dans debugfs

`debugfs: clri <numéro d'inode>`

- Lancement de la vérification du système de fichiers

`sudo fsck /dev/sda1`





















