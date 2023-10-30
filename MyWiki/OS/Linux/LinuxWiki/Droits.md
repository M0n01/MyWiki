
## DAC = Discretionary Access Control

Le contrôle d'accès sous les systèmes Unix, et par héritage sous Linux, est dit “**discrétionnaire**“ : tous les objets (répertoires, fichiers, processus, etc.) sont la propriété d'un compte utilisateur ou système, ainsi que d'un groupe de comptes utilisateurs et/ou système.

Les droits associés à ces objets sont donc définis pour :
- Le propriétaire de l'objet
- Le groupe propriétaire de l'objet
- Tous les autres : comptes utilisateurs et/ou système qui ne sont pas le compte et le groupe propriétaire.

>Un processus lancé par un utilisateur hérite de ses droits même si ce processus n'en a pas forcément besoin.

Seul root et le proprio du fichier peut modifier les droits de celui-ci.

### Les 3 bits

- **`r`** -> read
- **`w`**-> write
- **`x`** -> execute

Les droits sont exprimés :
- Pour **`u`** (user)
- Pour **`g`** (group)
- Pour **`o`** (others)

Expression générale des droits sur un objet à l'aide de 9 bits, répartis en 3 groupes de 3 dans l'ordre suivant :
1. **rwx pour u**
2. **rwx pour g**
3. **rwx pour o**

Exemple :
```bash
chmod u=rwx fichier1  # rwx pour le user
chmod g=rx fichier1   # rx pour le groupe
chmod o=r fichier1    # r pour les autres
```

On peut aussi les exprimé en décimal. Ces valeurs allant de 0 à 7 exprimant les 3 bits des permissions sont dites **octales** (8 caractères différents pour les exprimer).
**`- - -`** **->  `000`  ->  `0`**
**`- - x`** **->  `001`  ->  `1`**
**`- w -`** **->  `010`  ->  `2`**
**`- w x`** **->  `011`  ->  `3`**
**`r - -`** **->  `100`  ->  `4`**
**`r - x`** **->  `101`  ->  `5`**
**`r w -`** **->  `110`  ->  `6`**
**`r w x`** **->  `111`  ->  `7`**

Exemple :
```bash
chmod 665 fichier1  # rw pour user et group, rx pour others
```

Dans le cas où un fichier est accessible en lecture pour `others`, le répertoire dans lequel le fichier est situé doit avoir le bit **`x`** pour `others`. On dit que le bit **`x`** positionné sur le répertoire donne le droit de “**traverser**” le répertoire.
### Changer la  propriété d'un objet linux

Faire en tant que root
```bash
sudo chown charle:lucas fichier1 # user proprio devient charle mais le groupe proprio reste lucas
```

Pour changer que le groupe proprio
```bash
sudo chgrp antoine fichier1
```

Changer groupe et user proprio de manière récursive
```bash
sudo chown -R seb:seb *  # * pour tout ce qu'il y a ici
sudo chown -R seb:seb /directory  # autre exemple
```

### Les droits spéciaux

#### **`SetUID`** est un droit spécial permettant d'exécuter un fichier avec les droits de son propriétaire.

Exemple :
- Le fichier `/etc/shadow` est accessible uniquement par root. Un utilisateur lambda n'y a pas accès. Pourtant lorsqu'il change son mdp via la commande **`passwd`**, le fichier `/etc/passwd` est bien modifier.
- Lorsque l'on regarde la commande **`passwd`** dans `/bin/passwd`, on peut voir: **-rwsr-xr-x 1 root root**. 
- Le tag correspondant au **`x`** pour le compte proprio est remplacé par **`s`** . C'est le **SetUID** bit. Il permet, lorsque l'on exécute `/bin/passwd`, d'hérité des droits du compte proprio du fichier (de la commande).

#### **`Sticky Bit`** est un droit spécial permettant de gérer des espaces partagés.

Pour mettre Sticky Bit, il faut ajouter `1` devant les autres droits.
- Exemple:
```bash
chmod 1744 fichier1  # droit 744 + Sticky Bit
```
- Résultat: **-rwxr--r-T 1 seb seb**
- **`T`** -> Sticky Bit

Il permet d'éviter que des comptes ayant des droits sur un fichier ne puisse le supprimer.

