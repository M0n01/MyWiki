#logs #rsyslog 
## Consultez les répertoires des fichiers de traces de  `rsyslog`

Le répertoire **`/var`** contient toutes les données variables du système et notamment les fichiers de traces dans le sous-répertoire **`/var/log`**.

Chacun des processus du système proposant au noyau Linux de tracer ses activités déclenche le traitement de ses informations par un service particulier :**`rsyslog`**.

Exemple de fonctionnement :
- toutes les traces émises par le noyau Linux (que l'on appelle "kernel ring buffer") seront stockées dans un fichier particulier,
- toutes les traces concernant l'authentification des utilisateurs seront dans un autre fichier.

Autres fonctionnalités intéressantes :
- envoyer ces traces sur le réseau afin de centraliser toutes celles d'un parc de machines sur un même serveur de logs,
- gérer la rotation automatique des fichiers,
- utiliser des formats de dates très précis (jusqu'au millième de seconde).

>[!Info]
>Le démon **`rsyslog`** se paramètre à l'aide d'un fichier de configuration, en l'occurrence : le fichier **`/etc/rsyslog.conf`**.

Voir la liste des canaux de logs 
```bash
grep "/var/log" /etc/rsyslog.conf 
```
- La bonne idée est de redirigé ces traces sur un serveur centralisé

## Faire serveur de log centralisé
#### Sur le serveur (Debian)
Dans `/etc/rsyslog.conf`  (dé-commenter):
```bash
module(load="imudp")
input(type="imudp" port="514")
```
Redémarrer
```bash
systemctl restart rsyslog
```
Vérifier (port...)
```bash
ss -lptuen
```

#### Sur le client (CentOS)
Dans `/etc/rsyslog.conf` :
```bash
authpriv.* @ip_serv_deb:514 # @ = UDP, @@ = TCP
```
Redémarrer
```bash
systemctl restart rsyslog
```

#### Tester
Sur serveur Debian
```bash
tail -f /var/log/auth.log
```
Se connecter sur le serv CentOS et observer les logs sur le serv Debian

## Analysez les traces d’authentification
#### CentOS
```bash
grep LOGIN /var/log/secure # info sur login
grep sshd /var/log/secure # info sur ssh
```
- Ne fonctionne plus car maintenant les fichiers sont des binaires
- Depuis systemd, il faut utiliser journalctl

#### Debian
```bash
journalctl | grep LOGIN # info sur login
journalctl | grep sshd # info sur ssh
```

## Analysez l’initialisation des cartes réseau

>[!Info]
>La commande **`dmesg`** s'appuie sur les fichiers **`syslog`** pour Debian et **messages** pour CentOS en filtrant leur contenu sur les messages du noyau pendant le processus de démarrage de la machine.

#### Debian
Info noyau/matériel
```bash
grep e1000 /var/log/kern.log # e1000 = driver
```
Info sur la config
```bash
grep enp0s3 /var/log/syslog
```

#### CentOS
Info noyau/matériel
```bash
grep e1000 /var/log/messages # e1000 = driver
```
Info sur la config
```bash
grep enp0s3 /var/log/messages
```

### Relevez les redémarrages du système

>[!Info]
>La commande **`who`** permet de visualiser qui sont les comptes connectés à l’instant T (+ autres options permettant de relever par exemple la date du dernier redémarrage).
>La commande **`w`**(work) effectue en fait un condensé de plusieurs autres commandes permettant de relever notamment l'activité du processeur.

Qui est connecté
```bash
who
```
Affiche qui fait quoi
```bash
w
```





