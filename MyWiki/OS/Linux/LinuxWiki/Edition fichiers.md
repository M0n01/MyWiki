
Sur Linux, tout est fichier.

[Guerre **vi** vs **emacs**](https://en.wikipedia.org/wiki/Editor_war) 

## vi, vim, neovim

>[!Info]
>**vi** est un éditeur de texte en mode terminal pour les systèmes Unix, créé par Bill Joy en 1976. Ce programme fut porté à l'origine directement dans la première version de la branche BSD des Unix en 1978.

2 mode de fonctionnement de **`vi`**:
- mode **commande**
- mode **saisie**

Dans l'éditeur **`vi`** ce ne sont plus les commandes du shell mais les commandes **`vi`**.

[Cheat Sheet](https://www.atmos.albany.edu/daes/atmclasses/atm350/vi_cheat_sheet.pdf)

![[Pasted image 20231026222519.png]]

## emacs

**emacs** est bien plus qu'un éditeur de texte en mode terminal. 

>[!Info]
>Ce logiciel a été écrit à l'origine par Richard Stallman en 1976, puis repris par lui-même au début des années 80 en version C, avec notamment une gestion d'extensions très intéressante développée avec le langage [**lisp**](https://fr.wikipedia.org/wiki/Lisp).  

L'intégration du langage interprété **lisp** offre à **emacs** une modularité et une flexibilité importante, permettant à son utilisateur d'ajouter ou de configurer des fonctionnalités à la volée, sans avoir à recompiler le programme.

[Cheat Sheet](https://cheatography.com/bilus/cheat-sheets/emacs-cider/)

## nano

- les fonctionnalités d'édition sont classiques,
- pas de distinction entre mode commande et mode saisie,
- à l'ouverture de nano, vous êtes prêt à saisir,
- les touches de direction fonctionnent

## Les fichiers (copie, suppression, déplacement)

Les fichiers sont représentés dans le noyau Linux par des structures nommées **inode**.

>[!Info]
>Un fichier est une structure du langage C définie directement au niveau [du code noyau de Linux](https://github.com/torvalds/linux/blob/master/fs/inode.c). Cette structure est nommée inode. Elle peut se représenter par une **[liste de champs](https://en.wikipedia.org/wiki/Inode_pointer_structure#/media/File:Ext2-inode.svg)** illustrée généralement sous la forme d’un tableau.

Regardons de près l'inode, le fichier donc. On y voit ses informations (sa taille, son propriétaire et groupe propriétaire, ses permissions, la date de ses derniers accès (lecture, modification), etc.) :

![[Pasted image 20231026225914.png]]

Dans cet inode, il y a :
- **les adresses de blocs directes**. Ces 12 champs contiennent les adresses directes des données du fichier sur les périphériques de type blocs les contenant (disque dur, périphériques externes type clé USB, etc.) :

![[Pasted image 20231026230036.png]]

- **les adresses de blocs indirects**. Ces champs contiennent simplement des pointeurs vers d'autres inodes dont la totalité des champs est adressée pour des blocs de données.

L’adresse contenue dans la ligne 13 renvoie à un nouvel inode ou bloc de pointeurs. Ce nouvel inode contient, lui, **128 lignes.** Chacune des adresses contenues dans ces lignes renvoient vers un bloc de données. On dit que ces blocs de donnés sont indirects :

![[Pasted image 20231026230314.png]]

L’adresse contenue dans la ligne 14 renvoie à un nouvel inode ou bloc de pointeurs de **128 lignes.**  Chacune des adresses contenues dans ces lignes renvoient vers un nouvel inode contenant à nouveau 128 lignes. Et chaque adresse contenue dans chaque ligne renvoie vers un bloc de données. On dit que ces blocs de donnés sont doublement indirects :

![[Pasted image 20231026230520.png]]
Pour la ligne 15 on parle de triple redirection.

Les opérations classiques de Copie/Déplacement/Suppression de fichier sous Linux sont exécutées sur les inodes avec les commandes **cp/mv/rm**

Les **liens** durs/symboliques sous Linux représentent des pointeurs entre inodes et sont gérés avec la commande **`ln`**.

Voir les inodes
```bash
ls -li
```
On peut y voir plusieurs infos dont le nombre de références à un fichier

Créer un lien (dur) entre un fichier et un autre
```bash
ln fichier1 fichier2
```
ils auront donc le même numéro d'inodes. Quand on en modifie un l'autre est modifier aussi.

Créer un lien symbolique
```bash
ln -s fichier4 fichier5
```
Ainsi on crée fichier5 qui est un "raccourci" pour fichier4. Dans l'inode de fichier5 on a pas simplement un bloc de données, on a l'adresse du fichier vers lequel on pointe, ici fichier4.
`lrwxrwxrwx` -> le `l` signifie qu'il y a un lien symbolique