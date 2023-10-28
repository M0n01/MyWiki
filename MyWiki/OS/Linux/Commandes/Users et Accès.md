Un utilisateur est identifié par
- son uID = User Identification (un numéro)
- son gID = groupe principal (un numéro)
- ses GIDs = Groupes de travail

Il dispose :
- d'un répertoire de travail (/home/login)
- interpréteur de commande (/shell)

Droits sur Utilisateur ou Utilisateur Authentifier

### Utilisateurs et groupes
```bash
adduser nom_utilisateur  // ajouter utilisateur

addgroup nom_groupe      // ajouter groupe

id nom_utilisateur       // info utilisateur (uid,gid...)

usermod -G nom_groupe nom_user  // ajouter un utilisateur à un groupe secondaire

usermod -g nom_groupe nom_user // change le groupe principal (le groupe propriétaire lorsque le user créé un dossier ou fichier)

chgrp nom_groupe /repertoire    // changer groupe propriétaire de repertoire

gpasswd -a user groupe      // ajouter un user à un groupe 

cat /etc/group      // affiche la liste des groupes

cat /etc/passwd     // affiche la liste des utilisateurs

cat /etc/shadow     // affiche hash de mdp

visudo /etc/sudoers    // editer fichier de conf sudo
```

### Accès
```bash
chmod g+rwx /repertoire  // donner droits rwx au groupe sur repertoire
chmod u+rwx fichier      // droits pour utilisateur sur fichier
chmod a-rwx              // enlever droits pour tout le monde
chmod 1777 /repertoire   // groupe proprio de repertoire a tout les droits sauf supprimer, seul le créateur peut supprimer
chmod u+s       // ajouter bit s
```

### ACLs

ACL -> lis d'abord les droits utilisateur puis les droits de groupe puis les droits des autres
```bash
setfacl -m u:nom_utilisateur:rwx /repertoire  // pareil pour utilisateur

setfacl -m g:nom_groupe:rwx /repertoire  // droits rwx au groupe sur repertoire

setfacl -m d:u:nom_utilisateur:rwx /repertoire  // ACL par défaut (ACL heritage). ACL appliquer 

setfacl -m g:nom_groupe:0 /repertoire    // enlève tout les droits au groupe sur repertoire

setfacl -m o::0 /repertoire    // enlève tout les droits au autres

getfacl /repertoire  // liste les acl de repertoire

cat /etc/sudoers
```

UID : GID : Others
rwx : rwx : rwx
7 : 7 : 7

#### Bit s (substitute)

Bit de permission spécial qui peut être défini sur un fichier ou un répertoire dans un système de fichiers Linux.

Lorsque le bit s est défini sur un fichier, cela signifie que le processus exécutant le fichier aura les mêmes privilèges que l'utilisateur propriétaire du fichier. Si le bit s est défini sur un répertoire, cela signifie que tous les fichiers créés dans ce répertoire hériteront du groupe propriétaire du répertoire au lieu du groupe primaire de l'utilisateur qui les a créés.

L'utilisation du bit s peut poser des risques de sécurité, car elle permet à des processus d'exécuter des commandes avec des privilèges élevés. Utiliser le bit s avec précaution et uniquement lorsque cela est nécessaire.

#### Bit t (sticky)

Bit de permission spéciale qui peut être défini sur un répertoire dans un système de fichiers Linux.

Lorsque le bit t est défini sur un répertoire, cela signifie que seuls les propriétaires des fichiers ou des sous-répertoires contenus dans ce répertoire ont le droit de les supprimer. Cela empêche les utilisateurs malveillants de supprimer des fichiers ou des répertoires qui ne leur appartiennent pas dans un répertoire partagé.

#### Mask

Le masque de mode (umask) est un paramètre de configuration système qui permet de définir les permissions par défaut des nouveaux fichiers et répertoires créés sur un système de fichiers Linux.
Lorsque l'on crée un fichier, un masque de droits automatique s'applique.
Le masque par défaut est : umask 022
les fichiers seront donc créés avec les droits : 755

#### Bonnes pratiques










