### Stockage RAID 1 (ne fonctionne pas sous VMWare)
#### NE SURTOUT PAS REBOOT LA MACHINE PENDANT LA MANIPULATION

- Prérequis :
- 3 Disques (20Go, 1Go, 1Go)
- Un peu de neurones

-Il est nécéssaire que vos disques soit branchable à chaud

![image](https://user-images.githubusercontent.com/73076854/208864749-dc000b05-e5ea-4ba5-ab88-fb54c326eaa1.png)

- Installation des paquets
```bash
apt update
apt install mdadm lvm2
```
- Création du RAID 1
```bash
mdadm --create /dev/md127 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
mdadm --detail /dev/md127
mdadm --examine --scan --verbose
```
- Rebooter et verifier avec la commande 
```bash
cat /proc/mdstat 
```
- Créer un Volume Physique
```bash
pvcreate /dev/md127
pvdisplay /dev/md127
```
- Groupe de Volume
```bash
vgcreate vdisk1 /dev/md127
vgdisplay vdisk1
```
- Automatisation pour le démarrage une fois RAID et LVM configuré : 
```bash
vgchange -ay vdisk1
```
• Création des disques virtuels
```bash
lvcreate -L 100M -n lvol0 vdisk1
lvcreate -L 200M -n lvol1 vdisk1
lvscan
```
• Création des systèmes de fichiers
```bash
apt install ntfs-3g
mkfs.ext4 /dev/vdisk1/lvol0
mkfs.ntfs /dev/vdisk1/lvol1
```
• Monter les partitions
```bash
mkdir /mnt/linux
mkdir /mnt/windows
mount /dev/vdisk1/lvol0 /mnt/linux 
mount /dev/vdisk1/lvol0 /mnt/windows
```
- Créer un fichier dans ce repertoire
- Dans la configuration de la VM, supprimer un disque du RAID
- Constater : 
```bash
mdadm --detail /dev/md127
fdisk -l
mount
```
- Changement du Disque du Raid
- Recréer un disque de 1Go
```bash
apt install parted
partprobe
fdisk -l
mdadm --manage /dev/md127 --add /dev/sdc
mdadm --detail /dev/md127
```
- Si cela ne fonctionne pas
```bash
apt install parted
partprobe
fdisk -l
mdadm --stop /dev/md127
mdadm -A --force /dev/md127
mdadm --manage /dev/md1 --add /dev/sdc
mdadm --detail /dev/md127
```