> Gaël le Maréchal

#### Travail à faire :

1. Ouvrez un terminal et lancez un programme qui utilise beaucoup de mémoire, comme un navigateur web ou un éditeur de texte.

J'ai lancé Youtube sur Firefox pour utiliser plus de ressources.  

2. Utilisez les commandes précédente pour surveiller l'utilisation de la mémoire virtuelle et comprendre comment elle fonctionne.

Utilisation de `top`.
![[Pasted image 20231221095859.png]]

Utilisation de `free`.
Option `-m` (en Mo)
```bash
user@LabUTC502:~$ free -m
        total        used        free    shared   buff/cache   available
Mem:    3931        1054         170        47       2706        2574
Swap:   3023          36        2987
```
C'est plus lisible avec l'option `-h`
```bash
user@LabUTC502:~$ free -h
        total        used        free    shared  buff/cache   available
Mem:    3.8Gi       1.0Gi       153Mi      47Mi       2.6Gi       2.5Gi
Swap:   3.0Gi        36Mi       2.9Gi
```
Bonus : Utilisation de `htop` car c'est sympas.
![[Pasted image 20231221095758.png]]

Utilisation de `vmstat`
```bash
user@LabUTC502:~$ vmstat
procs -------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd free  buff cache   si  so    bi   bo   in   cs us sy id wa st
 0  0  37804 146976 82696 2601940 0   2   384  40   151  744  2  1 97  0  0
```

Avec toute ces commandes on a pu observer que le navigateur est en haut car c'est celui qui consomme le plus ressources. Ils sont classé par défaut par consommation de ressources CPU mais on peut ajouter des options pour les classé par consommation de d'autre ressources tel que le mémoire vive.
Il existe d'autre commande basé sur top permettant une meilleur lecture de l'attribution des ressources tel que `htop`ou `btop`.



