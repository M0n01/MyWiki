#réseau 
>[!Info]
>La configuration du réseau sous Linux est un aspect qui varie fortement entre les distributions.

## Configurer nom réseau de la machine

>[!Info]
>Le nom réseau d'un serveur Linux (aussi appelé "**system hostname**") sert d'identifiant par défaut de la machine pour tous les services et applications qui s'exécutent en local et pour beaucoup de services et d'applications lorsque ceux-ci communiquent sur le réseau.

- le fichier`/proc/sys/kernel/hostname` contient la valeur courante du nom réseau du serveur,
- la commande `hostnamectl` permet de la modifier,
- le fichier`/etc/hostname` permet d’initialiser le nom réseau au démarrage.

```bash
cat /proc/sys/kernel/hostname  # connaitre hostname
hostnamectl status             # infos hostname
```

Changer le hostname
```bash
hostnamectl set-hostname nouveaunom 
```
ou éditer directement le fichier `/etc/hostname`
```bash
nano /etc/hostname
```

## Interfaces réseaux

>[!Info]
>Avant toute configuration, il est nécessaire de s'assurer que le noyau Linux a bien détecté les cartes réseaux connectées d'une manière ou d'une autre sur la machine.

La commande **`dmesg`**(pour "display message") permet d'afficher les messages du noyau :
- pendant le processus de démarrage,
- et notamment lorsque ce dernier charge les pilotes des périphériques qui vérifient si un matériel connecté leur est compatible.

Détecter les cartes réseaux reconnues par le noyau lors du démarrage avec **`dmesg`** (display message)
```bash
sudo dmesg | grep Network
sudo dmesg | grep eth0
```

Ou regarder dans **`/sys`**
```bash
ls -l /sys/class/net/
```

### Configurer les cartes réseaux de manière dynamique

manière dynamique -> elles seront perdu au reboot

>[!Info]
>Le package **`iproute2`** remplace le package **`net-tools`** et fournit les commandes qui remplacent les précédentes (`ifconfig`, `netstat`).

la commande **`ip`** pour :
1. gérer les interfaces réseaux,
2. et notamment leur attribuer une adresse de manière dynamique.

Voir les cartes réseaux
```bash
ip link show
```
Démonter et remonter carte réseau
```bash
ip link set eth0 down
ip link set eth0 up
```
Obtenir toutes les infos sur les cartes réseaux
```bash
ip a
```
Ajouter adresse IP
```bash
ip addr add 192.168.1.23/24 dev eth0
```
Supprimer adresse IP
```bash
ip addr del 192.168.1.23/24 dev eth0
```

### Configurer interfaces réseaux

Pour configurer de manière statique les cartes réseaux de Linux, il est nécessaire de saisir les informations dans des fichiers de configuration qui vont être lus pendant le boot du serveur.

**DEBIAN**
>[!Info]
>Sous Debian et ses dérivés, le répertoire de configuration des interfaces réseaux est**`/etc/network`**.
>Le fichier **interfaces** permet de déclarer cette configuration de manière statique.

Config une carte en ip static:
Dans **`/etc/network/interfaces`**
```bash
auto eth0
iface eth0 inet static
address 192.168.1.23
netmask 255.255.255.0
```
Redémarrer
```bash
systemctl restart networking.service
```

**REDHAT**
>[!Info]
>Pour les distributions REDHAT et dérivés (notamment CentOS), le répertoire de configuration des cartes est `/etc/sysconfig/network-scripts/`. Ce répertoire contient notamment un fichier préfixé `ifcfg-` pour chaque carte reconnue par le système.

Config une carte en ip static:
Dans **`/etc/sysconfig/network-scripts/`** il y a 1 fichier par interface.
```bash
nano /etc/sysconfig/network-scripts/ifcfg-eth0
```
Config
```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none  # pour dhcp : none -> dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=eth0
UUID=41171a6f-bce1-44de-8a6e-cf5e782f8bd6
DEVICE=eth0
ONBOOT=yes
IPADDR=192.168.2.203
NETMASK=255.255.255.0
```

Redémarrer
```bash
systemctl restart network
```

### Configurer routes et passerelles

Utiliser la commande **`ip`** du package **`iproute2`** pour gérer les routes associées aux cartes réseaux du système. Cela permet par exemple de :
- lister les routes existantes,
- ajouter une passerelle supplémentaire,
- ou en modifier une configurée par défaut sous Debian et CentOS.

Voir les routes
```bash
ip route list
```
Enlever la route par défaut
```bash
ip route del default via 192.168.1.1
```
#### Ajouter une route par défaut
```bash
ip route add default via 192.168.1.1
```
PERMANENT -> via le fichier de conf : 
- **DEBIAN** -> `/etc/network/interfaces`
```bash
auto eth1
iface eth0 inet static
address 192.168.1.23
netmask 255.255.255.0
gateway 192.168.1.1
```
- **REDHAT** -> `/etc/sysconfig/network-scripts/ifcfg-eth0` 
```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none  # pour dhcp : none -> dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=eth0
UUID=41171a6f-bce1-44de-8a6e-cf5e782f8bd6
DEVICE=eth1
ONBOOT=yes
IPADDR=192.168.2.203
NETMASK=255.255.255.0

GATEWAY=192.168.1.1
```
#### Ajouter une route
```bash
ip route add 192.140.1.0/24 via 192.168.1.254 dev eth1
```
PERMANENT -> via le fichier de conf :
- **DEBIAN** -> `/etc/network/interfaces`
```bash
auto eth1
iface eth0 inet static
address 192.168.1.23
netmask 255.255.255.0
gateway 192.168.1.1
post-up ip route add 192.140.1.0/24 via 192.168.1.254 dev eth1
```
- **REDHAT** -> créer un fichier spécifique `/etc/sysconfig/network-scripts/route-eth1` 
```bash
default via 192.168.1.1 dev eth1
192.140.1.0/24 via 192.168.1.254 dev eth1
```

### Configuration DNS

En règle générale, il s'agit simplement d'indiquer dans un fichier de configuration les adresses IP des serveurs hébergeant les services de résolution de noms de domaine.

Cette configuration est identique entre les distributions **DEBIAN** et les distributions dérivées **REDHAT**. Cela se passe dans les fichiers **`/etc/hosts`** et **`/etc/resolv.conf`**  .

Dans **`/etc/nsswitch.conf`** 
```bash
hosts:          files dns
```
on peut voir la liste des configurations possible pour résoudre les noms et adresses IP. Ici on peut voir qu'il va d'abord configurer son fichier en local et après il fera une requête DNS.
- Ce fichier c'est **`/etc/hosts`**.

Ajouter une résolution en local:
- **`/etc/hosts`**
```bash
192.168.1.31 debServeur1
```
Ajouter une résolution via serveur DNS:
- **`/etc/resolv.conf`**
```bash
nameserver 192.168.1.20
```


