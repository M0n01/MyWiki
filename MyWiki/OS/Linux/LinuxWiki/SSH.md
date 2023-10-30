Les deux principaux algorithmes de cryptographie asymétrique sont **RSA** et **DSA**, et ils sont environ 1 000 fois plus lents que les algorithmes de asymétriques.

le protocole SSH se définit par l'utilisation des deux procédés de cryptographie :
1. asymétrique pour mettre en place un échange sécurisé avec le client,
2. et symétrique pour gérer les données.

**Le principe de fonctionnement est le suivant** :
1. Le serveur écoute les demandes de connexions entrantes
2. Un client demande une connexion, le serveur lui répond les algorithmes de chiffrement à sa disposition
3. Le client valide un algorithme et le serveur fournit au client sa clé publique
4. À partir de ce moment-là, le client peut vérifier que tous les messages qu'il va recevoir proviennent bien du serveur
5. Le client et le serveur échangent grâce à la cryptographie asymétrique pour s'accorder sur une clé de chiffrement symétrique basée sur un très grand nombre premier, on l'appelle la **clé de session SSH**
6. Une fois cette clé partagée, le client et le serveur peuvent l'utiliser pour tout le reste de la session.

>[!info]
>La brique logicielle **OpenSSH** fournit notamment les éléments suivants:
> - **`sshd`**: le service SSH 
> - **`ssh`**: le client SSH 
> - **`ssh-keygen`** et **`ssh-copy-id`**: les utilitaires de gestion de clé RSA, DSA 
> - **`scp`** et **`sftp`**: les clients permettant le transfert de données via SSH

Installer 
```bash
apt-get install openssh-server
```
Voir état 
```bash
systemctl status sshd
```
Voir les port ouverts (ici ssh)
```bash
ss -lptun | grep ssh
```
Lancer au démarrage
```bash
systemctl enable sshd
```

### Partie Client

Installer
```bash
apt-get install openssh-client
```
Ce package amène un binaire nommé "ssh".

Pour se connecter à distance en ssh depuis le client il suffit de lancer le programme en indiquant :
- le compte avec lequel l'authentification doit s'effectuer,
- l'adresse du serveur

Exemple
```bash
ssh seb@192.168.1.27
```
Voir les clés correspondantes aux authentification avec serveurs
```bash
cat .ssh/known_hosts
```
Connexion en spécifiant le port
```bash
ssh deb@192.168.1.27 -p numport
```

### Sécurisez la connexion SSH

- **Point faible** -> mot de passe
- **Solution** : Générer, en tant que client, notre propre doublon de clé privée/clé publique. Cela permettra notamment de diffuser cette clé publique sur les serveurs pour faciliter notre authentification avec la clé privée associée. Donc plus besoin de rentrer de mots de passes.

1) Générer clés
```bash
ssh-keygen -t rsa -b 4096 -f /root/.ssh/id_rsa -N ''
```
- -t : sélection de l'algorithme
- -b : taille de la clé
- -f : localisation et nom du fichier
- -N : pour ne pas protéger la clé privé avec un mdp (sinon il va falloir le rentrer à chaque connection)

Une clé publique (nommé .pub) sera créer au même endroit.

Diffuser clé publique
```bash
ssh-copy-id /root/.ssh/id_rsa.pub deb@192.168.1.27
```

Sur le serveur **deb** , dans `/home/deb/.ssh/authorized_keys` on peut voir la clé publique du client.
```bash
cat /home/deb/.ssh/authorized_keys
```

>[!Info]
>SSH est un sujet vaste sur linux. On peut faire beaucoup de chose comme les rebonds, les callbacks, les tunnels sur d’autres protocoles, etc.




