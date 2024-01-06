>Gaël le Maréchal

**Objectif :**

Comprendre le fonctionnement de la mémoire et de l'espace d'échange sous Linux, et comment configurer et utiliser ces ressources.

**Étapes :**

1. Vérifier la quantité de mémoire RAM disponible sur le système  
```bash
user@LabUTC502:~$ free -h
             total      used        free     shared  buff/cache   available
Mem:         3.8Gi       545Mi       889Mi    18Mi     2.4Gi       3.0Gi
Swap:        975Mi        30Mi       945Mi
```
3Go de mémoire vive disponible.

2. Configurer l'espace d'échange d'une taille de 1Go:
- Créer une partition d'échange
```bash
sudo fallocate -l 1G /swapfile # je créé un fichier swap nommé swapfile
```
- Changer les autorisations pour ce fichier pour que seul root puisse y accéder
```bash
sudo chmod 600 /swapfile
```
- Configurer la partition d'échange
```bash
sudo mkswap /swapfile
```
- Activer la partition d'échange  
```bash
sudo swapon /swapfile
```
- Vérifier que la partition d'échange est correctement configurée
```bash
user@LabUTC502:~$ sudo swapon --show
NAME      TYPE       SIZE  USED PRIO
/dev/sda5 partition  976M 30.1M   -2
/swapfile file      1024M    0B   -3
```

3. Modifier la taille de l'espace d'échange à 2Go :
On désactive la partition
```bash
sudo swapoff /swapfile
```
On modifie sa taille
```bash
sudo fallocate -l 2G /swapfile
```
On reconfigure l'espace d'échange
```bash
sudo mkswap /swapfile
```
On active la partition
```bash
sudo swapon /swapfile
```

4. Vérifier la quantité de mémoire utilisée après la modification de la taille de l'espace d'échange
```bash
user@LabUTC502:~$ free -h
               total    used       free     shared   buff/cache  available
Mem:           3.8Gi    546Mi      883Mi     18Mi      2.4Gi       3.0Gi
Swap:          3.0Gi    29Mi       2.9Gi

```
La mémoire n'a pas changé car l'espace d'échange n'affecte pas la mémoire vive, il se trouve sur la mémoire morte.
```bash
user@LabUTC502:~$ sudo swapon --show
NAME      TYPE      SIZE  USED PRIO
/dev/sda5 partition 976M 29.9M   -2
/swapfile file        2G    0B   -3
```
On peut voir que la taille de notre espace d'échange est désormais de 2G.

