>Gaël le Maréchal
## Manipulation 1 : Manipulation de fichiers

#### 1) Ajout groupes auditeurs et utilisateurs
```bash
sudo vi /etc/group
```
![[Pasted image 20231220103203.png]]

#### 2) créer le compte 'tintin'
1. Ajout d'une entrée (ligne) correspondant à l'utilisateur dans le fichier /etc/passwd. et une autre dans /etc/shadow.
```bash
sudo vi /etc/passwd
	tintin:x:128:128:,,,:/home/tintin:/bin/bash
```

```bash
sudo vi /etc/shadow
	tintin:
```
- Test
![[Pasted image 20231220111426.png]]
Je ne peux pas me connecter à l'utilisateur 'tintin'

2. Positionnement du mot de passe initial
```bash
sudo passwd tintin
New password: 
Retype new password: 
passwd: password updated successfully
```
- Test
![[Pasted image 20231220111555.png]]
Je peux me connecter mais je n'ai pas de répertoire `/home`.

3. Création du répertoire personnel de l'utilisateur et copie des fichiers d'environnement (les fichiers contenus dans /etc/skel).
```bash
cp --recursive /etc/skel /home/tintin
```
- Test
![[Pasted image 20231220111749.png]]
Je peux me connecter et j'ai le répertoire `/home`.
![[Pasted image 20231220111916.png]]
En revanche les droits ne sont pas bon.

4. Changement de propriétaire pour ce répertoire
```bash
sudo chown --recursive tintin /home/tintin 
sudo chmod --recursive 0755 /home/tintin
```
- Test
![[Pasted image 20231220112048.png]]
Les droits sont désormais correctes.

## Manipulation 2 : Commandes de base

1) **A quoi sert la commande adduser qu'elle est la différence avec useradd ?**
La commande `adduser` et `useradd` sont presque identiques, mais `adduser` est plus simple et convient mieux pour les utilisateurs inexpérimentés. 
Elle permet d'ajouter un nouveau utilisateur sans avoir à saisir les mêmes informations plusieurs fois. 
`adduser` ne permet pas de spécifier un nom d'utilisateur ou un mot de passe, il faut donc les fournir manuellement après avoir exécuté la commande.

2) **Est-ce que l’utilisateur « bin » existe ?**
```bash
id bin
	uid=2(bin) gid=2(bin) groups=2(bin)
```
Oui il existe.

3) **Existe-t-il d’autres comptes utilisateurs possédant les mêmes droits que « root » ?**
Non. Car dans le fichier `/etc/sudoers`:
```bash
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "@include" directives:

@includedir /etc/sudoers.d
```

4) **A quels groupes appartient l’utilisateur « bin » ?**
```bash
groups bin
	bin : bin
```
L’utilisateur "bin" appartient au groupes "bin".

## Manipulation 3 : Collecte des informations

1) Créer avec useradd en gardant les caractéristiques par défaut, l’utilisateur pierre. Quel est le groupe de pierre ? Son shell ? Son répertoire de travail ?

```bash
sudo useradd pierre
id pierre 
	uid=1001(pierre) gid=1001(pierre) groups=1001(pierre)
```
Le groupe de "pierre" est "pierre".

```bash
getent passwd pierre
	pierre:x:1001:1001::/home/pierre:/bin/sh
```
Son shell est "bash" et son répertoire de travail est `/home/pierre`.

2) Ajoutez un groupe staff. Enregistrez pierre dans ce groupe, tout en gardant ses autres groupes. Affichez les groupes de pierre.
```bash
sudo groupadd staff
sudo usermod -aG staff pierre
id pierre
	uid=1001(pierre) gid=1001(pierre) groups=1001(pierre),50(staff)
```

3) Connectez-vous au compte de pierre de deux façons différentes : à la connexion classique, et avec la commande su. Expliquez les différences. Que faut-il faire pour pouvoir se connecter normalement au compte pierre ? Faites le nécessaire.
```bash
sudo passwd pierre
New password: 
Retype new password: 
passwd: password updated successfully
```

```bash
su pierre
```
Cela fonctionne. Pour la connexion "classique" il faut ajouter "pierre" au groupe "vidéo".

4) Changez le champ commentaire de pierre en utilisant la commande vipw. Renseignez le champ avec le texte suivant : « Pierre Cerf – Lannion ».
```bash
sudo vipw
```

5) Verrouillez le compte de pierre. Essayez de vous connecter au compte pierre et notez les messages. Déverrouillez ensuite ce compte et essayez de vous reconnecter à pierre.
- Verouiller 
```bash
sudo passwd -l pierre
su - pierre
	Password: 
	su: Authentication failure
```
- Déverrouiller
```bash
sudo passwd -u pierre
su - pierre
```

6) Créez un compte avec l’outil d’administration (linuxconf, users-admin…). Créez un compte «admin » d’UID 0. Hérite t'il automatiquement de tous les droits? Testez et à défaut donnez lui. Il peut servir si, par exemple, on oublie le mot de passe de root.

## Manipulation 5 : Gestion des utilisateurs suite...

1) **Que fait la commande id ? Donner les étapes nécessaires pour fermer le compte d'un utilisateur en prenant soin de sauvegarder ses données.**

La commande `id` affichera les informations de l'utilisateur courant.
- Sauvegarder le répertoire de travail
```bash
sudo mv /home/pierre /backup
```
- Désactiver le compte
```bash
sudo usermod -L nom_utilisateur
```

## Manipulation 6 : Gestion des groupes

1) **Tester la commande newgrp. A quoi sert elle ? Cette commande est elle durable ? Les droits de l’utilisateur sontils impactés ? Qu’en est il lors de la création d’un nouveau fichier ?**

**Newgrp** change l'identifiant de groupe réel actuel à la valeur du groupe indiqué, ou au groupe par défaut défini dans `/etc/passwd` si aucun nom de groupe n'est fourni.
La commande `newgrp` n'est pas durable au-delà de la session actuelle.
Les droits de l'utilisateur, tels que définis dans le fichier `/etc/passwd`, ne sont pas affectés par `newgrp`.
Lorsqu'on créé un nouveau fichier après avoir utilisé `newgrp`, ce fichier hérite du groupe principal actuellement défini.

## Manipulation 7 : Gestion des mots de passe

1) L'administrateur peut-il connaître le mot de passe d'un utilisateur ? Peut-il le changer ? Si oui comment ?

Non il ne peut pas connaître le mot de passe d'un utilisateur.
Oui il peut le changer -> `sudo passwd nom_utilisateur`

## Manipulation 8 : Mise en place de Quota

**Commandes de base :**
1) Cherchez le(s) rôle(s) des différentes commandes suivantes : edquota, quota, repquota, quoaton, quotaoff, quotacheck.

Les quotas permettent de limiter la quantité d'espace disque qu'un utilisateur ou un groupe peut utiliser.

- **`edquota` :** Permet à un administrateur de modifier les quotas d'un utilisateur ou d'un groupe en éditant manuellement le fichier de configuration des quotas. 
- **`quota` :** Affiche les quotas d'utilisation de l'espace disque pour un utilisateur ou un groupe. 
- **`repquota` :** Affiche un rapport détaillé des quotas pour tous les systèmes de fichiers qui prennent en charge les quotas. 
- **`quotaon` :** Active les quotas sur les systèmes de fichiers qui les prennent en charge. 
- **`quotaoff` :** Désactive les quotas sur les systèmes de fichiers qui les prennent en charge.
- **`quotacheck` :** Vérifie l'intégrité des fichiers de quotas et les met à jour si nécessaire. Il crée également les fichiers de quota s'ils n'existent pas.

**Mise en place de quotas pour un utilisateur** :
Instaurez un quota « soft » et « hard » pour un utilisateur que vous aurez créé au préalable.
« soft » -> avertissement
« hard » -> bloque

Créer nouvel utilisateur : 
```bash
sudo adduser testquota
```

Éditer les quotas :
```bash
sudo edquota -u testquota
```

Mettez en place ce quota après avoir vérifié que l’utilisateur ne dépasse pas le quota. 
```bash
quota -u testquota
```

Remplissez le compte de l’utilisateur et observez ce qui se passe.
```bash
dd if=/dev/zero of=/home/testquota/fichier bs=1M count=1100
```

Refaites la manipulation en remplissant le compte avant d’instaurer le quota. Que se passe-t-il à la connexion de l’utilisateur après la mise en place du quota? Peut-il encore se connecter ?

L'utilisateur peut encore se connecter.

Peut-il détruire des fichiers pour libérer l’espace ?

L'utilisateur peut détruire des fichiers existants

Quelle est la signification d’un quota sur un groupe ?

Lorsque que l'on configure un quota pour un groupe, on définit des limites sur la quantité totale d'espace disque que tous les membres d'un' groupe peuvent utiliser collectivement sur un système de fichiers spécifique.

## Manipulation 9 : ACL

Vérifier que le noyau est bien configuré pour supporter les ACL (être root pour effectuer les commandes) :

Exécuter la commande suivante : grep ACL /boot/config-*
```bash
grep ACL /boot/config-*
	/boot/config-5.10.0-13-amd64:CONFIG_EXT4_FS_POSIX_ACL=y
    ...
	/boot/config-5.10.0-13-amd64:CONFIG_FS_POSIX_ACL=y
	/boot/config-5.10.0-16-amd64:CONFIG_FS_POSIX_ACL=y
	...
```
La ligne CONFIG_FS_POSIX_ACL=y est bien présente et celle lié au système de fichier également.

Installer le paquet ACL : apt-get install acl
```bash
sudo apt-get install acl
	Reading package lists... Done
	Building dependency tree... Done
	Reading state information... Done
	acl is already the newest version (2.2.53-10).
	acl set to manually installed.
	0 upgraded, 0 newly installed, 0 to remove and 11 not upgraded.
```

Créer un utilisateur Tintin, créer un fichier article donner les droits de lecture et d’écriture à Tintin sur ce fichier test avec ACL (utiliser la commande setfactl).
```bash
sudo adduser tintin2
	Adding user `tintin2' ...
	Adding new group `tintin2' (1003) ...
	Adding new user `tintin2' (1003) with group `tintin2' ...
	Creating home directory `/home/tintin2' ...
	Copying files from `/etc/skel' ...
	New password: 
	Retype new password: 
	passwd: password updated successfully
	Changing the user information for tintin2
	Enter the new value, or press ENTER for the default
		Full Name []: 
		Room Number []: 
		Work Phone []: 
		Home Phone []: 
		Other []: 
	Is the information correct? [Y/n] y

touch article
```
- ACL
```bash
setfacl -m u:tintin2:rw article
```

Tapez la commande getfacl afin de vérifier les droits nouvellement créé. Quels sont les droits de Tintin ? Créer un masque par défaut de lecture seule pour les autres utilisateurs.
```bash
getfacl article 
	# file: article
	# owner: user
	# group: user
	user::rw-
	user:tintin2:rw-
	group::r--
	mask::rw-
	other::r--
```
- Masque par défaut
```bash
setfacl -m o::r-- article
```

Créer le repertoire PetitReporter et appliquer les droits de lecture écriture sur tout le repertoire pour Tintin.
```bash
mkdir PetitReporter
setfacl -m u:tintin2:rwx PetitReporter/
```
J'ai ajouté le droit d’exécution pour qu'il puisse "traverser" le dossier.

Quelle commande utiliser pour l’héritage afin que chaque fichier nouvellement créés aient bien les droits lectures écriture pour l’utilisateur Tintin et aucun droits pour les autres ?
```bash
setfacl -m d:u:tintin2:rw-,d:o::--- PetitReporter
```


Tester la commande setfacl avec l’option b sur le fichier article, à quoi sert elle?
```bash
setfacl -b article
getfacl article 
	# file: article
	# owner: user
	# group: user
	user::rw-
	group::r--
	other::r--
```
- L'option `-b` permet de supprimer toute les ACLs.

Quelle est la commande pour supprimer uniquement l’ACL d’un utilisateur ?
```bash
setfacl -x u:tintin2 article
```




