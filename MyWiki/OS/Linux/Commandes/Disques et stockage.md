### Stockage
voir [[Mémoire]]
```bash
lsblk      // info disques et partitions

ls -l /dev/disk/by-uuid/    // identifier les UUID des partitions

df -hT   // liste ce qui est monté

fdisk -l         // info disques et partitions
fdisk /dev/sdb   // monter nouvelle partition sur le disque sdb
>m       // help
>n       // ajouter partition
>p       // primaire
>+500    // de 500 Mo
>t       // modifier type
>0c      // FAT32 LBA
>w       // sauvegarder

mkfs.vfat /dev/sdb1   // formate sdb1 en FAT32
mkfs.ext4 /dev/sdb1   // formate sdb1 en ext4

mount /dev/sdb1 /repertoire  // monter partition dans repertoire

cat /etc/fstab   // table de montage (option noexec, noauto...)

mount -a -o remount    // remonte tout ce qui est déclaré dans le fstab

mount -o remount chemin    // remonte la partition chemin
```
- Pour déplacer un dossier existant dans une nouvelle partition voir [[Déplacé dossier dans nouvelle partition|procédure]].

#### Disques virtuels
voir [[Disques et partitions#Volumes logiques|Wiki]]
```bash
pvs   // liste les PV

vgs   // liste les VG

lvs   // liste les LV

pvdisplay /dev/sda2          // plus de détails sur le PV
vgdisplay systemvol          // plus de détails sur le VG
lvdisplay /dev/systemvol/<LV au choix>     // plus de détails sur le LV

'Procédure partitionnement virtuel :'

growpart /dev/sda 2     // met le reste d’espace disque dans la partition sda2
pvresize /dev/sda2      // adapte la taille de pv à l’espace disponible
lvextend -L 2G -r /dev/systemvol/home      // 2Go pour le LV home
lvextend -L +2G -r /dev/systemvol/home     // augmente le LV de 2Go
```
