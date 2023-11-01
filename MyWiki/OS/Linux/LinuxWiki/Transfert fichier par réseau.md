
La couche graphique est inutile : elle représente des failles potentielles de sécurité supplémentaires, et prendra des ressources matérielles pour rien.

- Les deux logiciels les plus utilisés pour télécharger des fichiers en HTTP sur le réseau : **wget** et **curl**.
- Transférer des fichiers d'un serveur à l'autre de manière sécurisée : **SSH**.
- Se connecter et transférer des fichiers à partir d'un client **FTP/SFTP**.

## Téléchargez sur internet avec  `wget`  et  `curl`

Les utilitaires **`curl`** et **`wget`** -> téléchargement de fichiers hébergés sur Internet ou via des services HTTP ou FTP. 
Aussi possible de transférer des fichiers d'un serveur ne disposant que du service SSH. Pour cela -> un autre logiciel de la brique OpenSSH :**`scp`**.

>[!Info]
>L'objectif de **`scp`** est de fournir une fonctionnalité de transfert de fichier sécurisée en s'appuyant sur le protocole SSH. 
>**`scp`** va utiliser le client **ssh**. Seule condition de son utilisation -> posséder un compte de connexion et un service SSH en écoute.

Commande scp :
```bash
scp fichier-source fichier-destination
```
le fichier-source ou destination peuvent être distant.
Exemple :
```bash
scp monfichier.txt seb@debserv:/home/seb/
```
Avec un port spécifique:
```bash
scp -P ....
```
Pour transférer tout un répertoire (récursif)
```bash
scp -R ...
```

## Transférez des fichiers par FTP/FTPS/SFTP

Le protocole SFTP s'appuie sur SSH, il est donc nécessaire d'installer un service SSH. SFTP n'utilise pas le protocole FTP. Un service FTP n'est donc pas nécessaire.

>[!Info]
>Il est nécessaire de coupler le serveur FTP avec un certificat **TLS/SSL,** pour obtenir un service **FTPS**.
>Par contre, sur un serveur disposant d'un accès SSH, il est possible de faire du “**FTP par SSH**”, aussi nommé **SFTP**.

1. installer le logiciel FileZilla.
2. L’utiliser pour effectuer des transferts de fichier via FTP, SFTP.

- Dans Host : sftp://192.168.1.27
- Username : seb
- Password : motdepasse de seb
- Port : 22   

Pour faire la connection via clé:
- Edit -> settings
- Ajouter dans SFTP la clé privé