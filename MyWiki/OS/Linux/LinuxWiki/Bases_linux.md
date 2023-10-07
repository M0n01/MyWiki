#linux #wiki 

La philosophie des systèmes UNIX/Linux, c’est : Faire une chose et la faire parfaitement.

Linux est un système d’exploitation monolithique modulaire.

**Monolithique** : Cela veut dire que tout le code de Linux est exécuté dans UN SEUL gros objet en mémoire de l’ordinateur. C’est ce qu’on appelle le noyau.
Le noyau contient toutes les fonctions rendues par le système d’exploitation, comme par exemple la gestion de la mémoire, du processeur, des disques, etc.

**Modulaire** : Cela veut dire que le code du noyau Linux est organisé sous la forme de modules. Ce sont des blocs de code. 

![Alt text](image-2.png)

Grâce à cela, on peut ajouter et/ou retirer des modules dans un noyau en cours d’exécution, sans avoir à l’arrêter ou à le redémarrer.

Code du noyau : https://www.kernel.org/

**Version noyau linux**
![Alt text](image-1.png)

- X est le numéro majeur, il correspond à ce qu’on pourrait appeler une génération : il s’agit d’une version qui lui fait franchir un cap important.
- Y est le numéro mineur, il évolue plus rapidement au sein d’une branche majeure, il correspond à toutes les mises à jour régulières du code au sein d’une génération.
- Z correspond à une révision, c'est-à-dire une correction d’anomalie ou une mise à jour de sécurité. 

### Les distributions

Une distribution Linux se compose d’un noyau, de packages, et d’outils pour gérer leurs dépendances. Elles sont développées pour répondre à un besoin (serveur, poste de travail ou autre).

Lorsque tous ces packages répondant à un ou plusieurs besoins sont réunis, ils forment ce que l’on appelle une distribution Linux.

![Alt text](image-3.png)

### Les bureaux

C’est une interface graphique qui réagit par rapport à ce qu'on fait avec le clavier ou la souris,  comme cliquer sur une croix pour fermer une fenêtre. Les environnements de bureau sous Linux sont modulaires.

Chaque module est responsable d’une fonction (authentification, gestion des fenêtres, du fond d’écran, etc.).

>[!Remarque]
>Gnome, KDE et XFCE sont les 3 bureaux les plus connus de Linux.

## Administration

La console sous Linux est un périphérique gérant le clavier et l'écran de l'ordinateur et propose d'interagir avec l'utilisateur via un terminal en mode texte. 
À vrai dire, la console de Linux propose 7 terminaux en mode texte, appelés aussi les terminaux physiques. Ils sont directement sur le clavier branché à l'ordinateur et disponibles à partir des combinaisons de touches : “CTRL+ALT+F1” ; “CTRL+ALT+F2” ; … jusqu’à “CTRL+ALT+F7”.

Chacune de ces combinaisons de touches propose l'émulation d'un terminal (en mode console) différent sur lequel il est possible de se connecter de manière indépendante avec un compte utilisateur différent.

Dans la grande majorité des cas, on se connecte à distance sur un serveur Linux via un émulateur de terminal. Il s’agit d’un programme lancé sur un poste de travail Windows/Mac ou même Linux. Il gère la connexion au serveur distant avec un protocole réseau (telnet, rlogin ou SSH).
- PuTTY sous Windows
- Terminal sous macOS
- xterm, Konsole, GNOME Terminal... sous Linux

#### Shell

**Shell** : Interpréteur de commandes. C'est un programme qui est exécuté lors de la connexion de l'administrateur sur une console ou un terminal. Il présente une interface en mode texte qui permet de saisir des commandes.

![Alt text](image-4.png)

Le rôle principal du shell est d'exécuter les commandes saisies par l'administrateur lui permettant d'effectuer des appels systèmes vers le noyau.

>[!Hisoire]
>En 1977, Stephen Bourne écrit le Bourne shell qui devient une référence en matière d'interpréteur de commandes.

Sous Linux, le shell standard est le Bash (pour Bourne Again Shell). C'est une variation avancée de Bourne shell.

*Les distributions BSD implémentent plutôt C Shell par défaut.*

Le shell exécuté lors de la connexion d'un utilisateur sur un terminal est configuré dans le fichier  /etc/passwd  

Le programme shell intègre : 
- Un interpréteur de commandes.
- La gestion des canaux entrée/sortie/erreur.
- Un langage de programmation.



