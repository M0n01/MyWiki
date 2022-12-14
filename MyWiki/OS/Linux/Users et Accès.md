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

usermod -G nom_groupe nom_user  // ajouter un utilisateur à un groupe

chgrp nom_groupe /repertoire    // changer groupe propriétaire de repertoire

gpasswd

cat /etc/group      // affiche la liste des groupes

cat /etc/passwd     // affiche la liste des utilisateurs

cat /etc/shadow

visudo /etc/sudoers    // editer fichier de conf sudo
```

### Accès (ACLs)
```bash
chmod g+rwx /repertoire  // donner droits rwx au groupe sur repertoire
chmod u+rwx fichier      // droits pour utilisateur sur fichier
chmod a-rwx              // enlever droits pour tout le monde
chmod 1777 /repertoire   // groupe proprio de repertoire a tout les droits sauf supprimer, seul le créateur peut supprimer

setfacl -m g:nom_groupe:rwx /repertoire  // droits rwx au groupe sur repertoire
setfacl -m u:nom_utilisateur:rwx /repertoire  // pareil pour utilisateur
setfacl -m d:u:nom_utilisateur:rwx /repertoire  // ACL par défaut
setfacl -m g:nom_groupe:0 /repertoire    // enlève tout les droits au groupe sur repertoire

getfacl /repertoire  // liste les acl de repertoire

cat /etc/sudoers
```

ACL -> lis d'abord les droits utilisateur puis les droits de groupe puis les droits des autres


UID : GID : Others
rwx : rwx : rwx
7 : 7 : 7

#### Bit s (substitute)


#### Bit t (sticky)

#### Mask


#### Bonnes pratiques










