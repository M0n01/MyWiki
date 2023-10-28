#arborescence #fichiers

[**FHS**](https://refspecs.linuxfoundation.org/fhs.shtml) = Norme arborescence Linux. Référence l'objectif primaire de tous les répertoires standard du système Linux.

- `/boot` : Noyau Linux
- `/bin` : Commandes (binaires)
- `/etc` : Fichiers de configuration
- `/home` : Stock les fichiers des comptes personnels des users. Donc dans un serveur WEB pas besoin de /home. Mais sur un serveur de fichiers oui.

>[!Attention]
>`/root` ne doit pas être identifier à un compte "humain".
>Pour des raisons de sécu : Ne rien stocker dans `/root`

- `/proc` et `/sys` : N'existe pas sur le disque. Linux le maintien en mémoire. Permet d'intervenir à chaud sur comportement du système.

**Répertoires marqués "shareable"** : Répertoires contenant des informations stockées sur un équipement mais utilisées par d'autres équipements. Comme `/home`.

**Répertoires marqués "unshareable"** : Répertoires contenant des fichiers qui ne peuvent pas ou ne doivent pas être partagés entre plusieurs équipements. Comme `/boot`.

**Répertoires marqués "static"** : Répertoires contenant des données qui ne peuvent pas changer d'elles-mêmes, ou sans l'intervention de l'administrateur, avec nécessité d'une élévation de privilèges. Comme `/boot` ou `/etc`.

**Répertoires marqués "variable"** : Inverse de "static". Comme `/home` ou `/var/www`.

Chaque répertoire sous la racine `/` possède deux fichiers spéciaux nommés `.` pour se  désigner soi-même, et `..` pour désigner son parent dans l'arborescence.

![[Pasted image 20231014122423.png]]

![[Pasted image 20231014122445.png]]
#### usr
- **`/usr/bin`**: ce répertoire contient majoritairement les commandes à destination de tous les utilisateurs du système, privilégiés ou non.
- **`/usr/sbin`**: ce sont des commandes à nouveau à destination unique de l'administrateur, mais non critiques pour le bon fonctionnement du système.c
- **`/usr/local/bin`** : Il va contenir tous les binaires qui sont compilés manuellement par l'administrateur après l'installation du système.
**`/usr`** est très important, il est marqué "**shareable**" et "**static**".

#### var
L'objectif de ce répertoire est simple : stocker toutes les informations utilisateurs, administrateurs et systèmes **variables**.
- **`/var/log`**: répertoire contenant l'arborescence de toutes les traces systèmes et applicatives. C'est dans ce répertoire qu'il est possible de consulter les traces des historiques système. Généralement les applications installées sur le système disposent de leur propre sous-répertoire (`/var/log/apache2`par exemple).
- **`/var/run`**: répertoire contenant toutes les données relatives aux processus en cours d'exécution.
- **`/var/spool`**: répertoire contenant des données stockées de manière temporaire entre processus. Souvent, ce répertoire est utilisé pour stocker des données relatives à des actions ou tâches à effectuer dans un futur proche par les processus en cours d'exécution.
- **`/var/mail`**: c'est le répertoire de stockage des messageries électroniques locales des comptes utilisateurs du système.

### Systèmes de fichiers virtuels

Il est possible de changer certaines variables du noyau, et donc certains comportements du système global en modifiant le contenu de ces fichiers “virtuels” placés dans ces arborescences. Ces arborescences n'existent pas sur périphérique physique de type bloc, et l'analyse indépendante du disque d'un système Linux ne les fera pas apparaître. Elles existent uniquement parce que le noyau les propose.

#### Le répertoire  `/proc`
Cette arborescence contient toutes les informations concernant le processus.
`/proc` présente aussi des fichiers et des répertoires.
>Ces fichiers étant virtuels, ils ont une taille de 0.
```bash
~$ file /proc/cpuinfo
/proc/cpuinfo: empty
```

D'autres fichiers sont intéressants à relever dans cette arborescence :
- **`/proc/version`** : contient la version exacte du noyau en exécution, 
- **`/proc/meminfo`** : les informations détaillés sur la mémoire vive gérée par le noyau,
- **`/proc/uptime`** : le temps d'exécution cumulé,
- **`/proc/cmdline`** : les paramètres passés au démarrage du noyau, etc.

**`/proc`** contient également beaucoup de répertoires, dont la grande majorité porte des noms à base de chiffres. En effet, tous les processus en exécution sur le système sont identifiés par un numéro unique géré par le noyau.
**`/proc/1`** est le premier processus du noyau.

#### Le répertoire  `/sys`
Cette arborescence fonctionne sur le même principe que `/proc`. Elle présente des informations maintenues en temps réel par le noyau. À une différence fondamentale près : certains éléments de cette arborescence sont accessibles en écriture aux comptes privilégiés du système et notamment les variables systèmes du noyau dans le répertoire **`kernel`**.

Cette arborescence contient les informations sur les périphériques gérés par le noyau, notamment :
- **`/sys/block`** ou **`/sys/dev`** les périphériques de type bloc ou caractères,
- **`/sys/devices`** les drivers,
- **`/sys/fs`** les différents systèmes de fichiers,
- **`/sys/module`**, les modules du noyau.

**`/sys/kernel`** contient une arborescence de fichiers représentant des variables du noyau accessibles en écriture et permettant de modifier le comportement à chaud du système. Par exemple, le répertoire `/sys/kernel/debug` contient des fichiers permettant d'activer des fonctions de traces et de débogage du noyau.