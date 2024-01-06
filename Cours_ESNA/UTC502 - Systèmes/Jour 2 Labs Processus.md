
### 1. Les processus

#### 1.1. Manipulations de base

1) Lancez un éditeur de texte en tâche de fond avec la commande :

> $ mousepad&

Regardez la liste de processus pour identifier le nouveau processus lancé correspondant (utilisez la commande 'ps -u'):
```bash
ps -u
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
user        1494  0.0  0.0   8112  3492 pts/0    Ss+  10:48   0:00 bash
user        1503  0.0  0.0   8112  3588 pts/1    Ss+  10:48   0:00 bash
user        1505  0.0  0.0   8112  3620 pts/2    Ss+  10:48   0:00 bash
user        1513  0.0  0.0   8112  3448 pts/4    Ss+  10:48   0:00 bash
user        1567  0.0  0.1   8112  4856 pts/5    Ss+  10:50   0:00 bash
user        2509  0.0  0.1   9204  5932 pts/3    Ss   12:35   0:00 bash
user        3428  0.2  0.9 459124 38400 pts/3    Sl   14:30   0:00 mousepad
user        3438  0.0  0.0   9700  3336 pts/3    R+   14:30   0:00 ps -u
```

2) Décrivez l'ensemble des champs affichés par cette commande. A quoi correspondent-ils ?

Prenons comme exemple la ligne suivante :
`USER    PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND`
`user    1505  0.0  0.0   8112  3620 pts/2    Ss+  10:48   0:00 bash`

- **user** : Le nom de l'utilisateur propriétaire du processus.
- **1505** : PID (Identifiant du processus) est un identifiant unique attribué à chaque processus en cours d'exécution sur le système.
- **0.0** : %CPU (Utilisation du CPU) : Pourcentage d'utilisation actuelle du CPU par le processus. 
- **0.0** : %MEM (Utilisation de la mémoire) : Pourcentage d'utilisation actuelle de la mémoire par le processus.
- **8112** : VSZ (Taille virtuelle) : La taille totale de la mémoire virtuelle utilisée par le processus, en kilo-octets.
- **3620** : RSS (Taille de la mémoire résidente) : La taille de la mémoire résidente utilisée par le processus, en kilo-octets.
- **pts/2** : TTY (Terminal associé) : Le terminal associé au processus. Ici, le processus est associé à un terminal pseudo (`pts/2`).
- **Ss+** : STAT (État du processus) : Ici,
        - `S`: Le processus est en cours d'exécution.
        - `s`: Le processus est un leader de session.
        - `+`: Le processus a été modifié par rapport à la ligne de commande du processus associé.
- **10:48** : START (Heure de démarrage) : L'heure à laquelle le processus a été lancé.
- **0:00** : TIME : Le temps CPU cumulé utilisé par le processus depuis son démarrage.
- **bash** : CMD (Commande) : La commande ou le programme qui a lancé le processus. Ici, c'est le shell Bash.

3) Rechercher et décrivez les différents états d'un processus linux

- **R (Running - En cours d'exécution) ->** Le processus est actuellement en cours d'exécution ou en attente d'exécution. 
- **S (Sleeping - En attente) ->** Le processus est actuellement en attente d'un événement, comme une interruption d'E/S (Entrée/Sortie) ou une minuterie. 
- **D (Uninterruptible Sleep - Sommeil non interruptible) ->** Le processus est dans un état de sommeil non interruptible, généralement en attente d'une opération d'E/S bloquante. Cela peut également indiquer un problème avec le système de fichiers. 
- **Z (Zombie) ->** Le processus a terminé son exécution, mais ses informations de statut n'ont pas encore été récupérées par son parent. Les processus zombies sont généralement temporaires et sont nettoyés par le système. 
- **T (Stopped - Arrêté) ->** Le processus est actuellement arrêté, soit par un signal tel que SIGSTOP, soit par le débogueur. 
- **t (Traced - Tracé) ->** Le processus est en mode de tracé, généralement à des fins de débogage. 
- **W (Paging - En attente de pagination) ->** Le processus est en attente de l'achèvement d'une opération de pagination. 
- **X (Dead) ->** Le processus est mort.

4) Sur l'image suivante, nous pouvons constater que le processus mousepad à 14h47. Il est maintenant 15h, pourtant le temps total d'exécution est d'environ 1 minute !? Comment cela est-il possible ?
Car il s'agit du temps d'utilisation du processus. Le temps CPU est un indicateur de l'utilisation des ressources du processeur, mais il ne mesure pas nécessairement la durée écoulée dans le monde réel.

5) La commande "ps -ejf" permet d'afficher le PPID (pid du père d'un processus). Utilisez cette commande pour identifier l'arbrorescence de l'ensemble des processus parent de mousepad. _Note : vous pouvez également utiliser la commande : pstree -p_

![[Pasted image 20231220150020.png]]



