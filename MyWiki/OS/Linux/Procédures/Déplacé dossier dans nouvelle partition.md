
### Déplacé /home dans nouvelle partition :
- renommé home
```bash
mv /home /home.old
```
- créer nouveau répertoire home
- créer une nouvelle partition de 1 Go
```bash
fdisk /dev/sdb   
>m       // help
>n       // ajouter partition
>p       // primaire
>+1G     // de 1G
>w       // sauvegarder
```
- la formater au bon file system
```bash
mkfs.ext4 /dev/sda3
```
- la monter dans le nouveau répertoire home
```bash
mount /dev/sda3 /home
```
- copier le contenu de home.old dans le nouveau home
```bash
cp -rp /home.old/* /home
```
**Monter la partition au démarrage de la machine pour éviter de refaire les manipulations**
- identifier l'UUID de la partition
```bash
ls -l /dev/disk/by-uuid/
``` 
- Modifier le fstab (monte les partitions)
- faire une copie par précaution
```bash
sudo cp /etc/fstab /etc/fstab.old
```
- le modifier
```bash
nano /etc/fstab
```
- ajouter la ligne à la fin
```bash
UUID=Mon_UUID /home ext4 default 0 2
```
