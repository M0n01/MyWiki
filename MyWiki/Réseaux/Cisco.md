
## Syntaxe des commandes

Le texte en **gras** signale les commandes et mots-clés à saisir tels quels.

Le texte en *italique* signale les arguments pour lesquels des valeurs doivent être saisies.

Les crochets signalent un élément facultatif (mot-clé ou argument).
```
[x] 
```
  

Les accolades signalent un élément requis (mot-clé ou argument).
```
{x} 
```
  
Les accolades et les lignes verticales encadrées par des crochets signalent un choix obligatoire, au sein d'un élément facultatif. Les espaces sont utilisés pour délimiter clairement les parties de la commande.
```
[x {y | z }] 
```

Info sur une commande
```
description
```

Aide contextuelle (peut aussi se mettre à la fin d'une commande quand on est pas sur de la suite)
```
?
```

### Raccourcis clavier

Effacer tous les caractères à partir du curseur jusqu'à la fin de la ligne de commande
- Ctrl+K 

Effacer tous les caractères à partir du curseur jusqu'au début de la ligne de commande.
- Ctrl+U ou Ctrl+X

Déplace le curseur vers le début de la ligne
- Ctrl+A

Déplace le curseur vers la fin de la ligne
- Ctrl+E

Séquence d'interruption permettant d'abandonner les recherches DNS, traceroutes, pings, etc
- Ctrl+Maj+6
ou
- Ctrl+Maj+9

## Commandes

>Au démarrage :
>initial configuration dialog ? NO

### Nom de périphériques

*Débutent par une lettre;
Ne contiennent pas d'espaces;
Se terminent par une lettre ou un chiffre;
Ne comportent que des lettres, des chiffres et des tirets;
Comportent moins de 64 caractères.*

Changer le nom
```
hostname nouveau_nom
```

Revenir au nom par défaut
```
no hostname
```

### Sécuriser l'accès en mode d'exécution utilisateur (quand on rentre dans le terminal)

Spécifier l'interface
```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# line console 0
```
- Le zéro sert à représenter la première (et le plus souvent, la seule) interface de console.

Spécifier mot de passe
```
Sw-Floor-1(config-line)# password motdepasse
```

Activer accès
```
Sw-Floor-1(config-line)# login
Sw-Floor-1(config-line)# end
```
- La console d'accès requiert à présent un mot de passe avant d'accéder au mode d'exécution utilisateur.

### Sécuriser l'accès en mode d'exécution (enable, le plus important)

```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# enable secret motdepasse    // Spécifie le mot de passe
  
Sw-Floor-1(config)# exit
```

### Sécuriser l'accès aux lignes VTY

>Les lignes VTY (terminal virtuel) activent l'accès à distance au périphérique en utilisant Telnet ou SSH.

Spécifier les lignes VTY, il y en a 16 (de 0 à 15)
```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# line vty 0 15 // Pour les lignes VTY de 0 à 15
```

Spécifier mdp
```
Sw-Floor-1(config-line)# password motdepasse
```

Activer accès VTY
```
Sw-Floor-1(config-line)# login 
Sw-Floor-1(config-line)# end
```

### Chiffrer les mots de passes

>Les fichiers startup-config et running-config affichent la plupart des mots de passe en clair. C'est une menace à la sécurité dans la mesure où n'importe quel utilisateur peut voir les mots de passe utilisés s'il a accès à ces fichiers.

Chiffrer tous les mots de passe non chiffrés.
```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# service password-encryption
```

Pour vérifier que les mdp sont bien chiffrés
```
Sw-Floor-1(config)# show running-config
```

### Bannière (avertissement)

```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# banner motd #Seulement pour les accès autorisé# // MOTD (Message Of The Day)
```

ou

```
R1(config)# banner motd #

Enter TEXT message. End with a new line and the #

*********************************************** 

Seulement pour les accès autorisé

***********************************************

R1(config)#
```

>Une fois cette commande exécutée, la bannière s'affiche lors de toutes les tentatives d'accès au périphérique jusqu'à ce qu'on la supprime.

### Configurations

Deux fichiers système stockent la configuration des périphériques:

startup-config 
```
show startup-config
```

>Fichier de configuration enregistré qui est stocké dans NVRAM (mémoire vive non volatile contenant les commandes utilisées au démarrage/redémarrage. Ne perd pas son contenu lors de la mise hors tension).

running-config
```
show running-config
```

>Stocké dans la mémoire vive (RAM). Reflète la configuration actuelle. Mémoire volatile (perd tout son contenu lorsque le périphérique est mis hors tension).

Enregistrer les modifications apportées à la configuration en cours dans le fichier de configuration initiale :
```
copy running-config startup-config
```
ou
```
copy run start
```

Pour appliquer la config startup à la config en cours (charger la sauvegarde antérieur) :
```
copy startup-config running-config
```
ou
```
copy start run
```

Restaurer la configuration en cours (redémarre) :
```
reload
```

Restaurer la configuration initial :
```
erase startup-config
```
Puis
```
reload
```
Pour recharger le périphérique pour supprimer le fichier de configuration en cours dans la mémoire vive.

>Lors du rechargement, un commutateur charge la configuration initiale qui était proposée à l'origine avec le périphérique.

Lister les directory : 
```
dir ?
```

Entrer dedans : 
```
dir nvram:
```


### Interface virtuelle switch

>Pour accéder à distance au commutateur, une adresse IP et un masque de sous-réseau doivent être configurés sur l'interface SVI. 

Pour configurer une SVI sur un commutateur :
```
interface vlan 1
```
Vlan 1 n'est pas une interface physique réelle mais une interface virtuelle. 

Attribuez ensuite une adresse IPv4 : 
```
ip address adresseIP masque
```

Enfin, activez l'interface virtuelle : 
```
no shutdown
```

### Info réseau

> Voir [[MyWiki/OS/Windows/Commandes de base#Réseau|Réseau Windows]]

Désactive les diffusions dirigées
```
no ip directed-broadcasts
```

Info interface
```
show interface F0/1
```

Affiche la table d’adresse MAC
```
show mac address-table
```

Clear la table
```
clear mac address-table dynamic
```

Affiche la table ARP
```
show ip arp
```

Affiche la table de routage sur le routeur
```
show ip route
show ipv6 route
```

Affiche adresse des interfaces Ethernet
```
show ipv6 interface brief
```

>Notez que chaque interface possède deux adresses IPv6. La deuxième adresse de chaque interface est la GUA configurée. La première adresse, celle qui commence par FE80, est l'adresse de monodiffusion link-local de l'interface. Souvenez-vous que l'adresse link-local est automatiquement ajoutée à l'interface lorsqu'une adresse de diffusion globale est attribuée.

>Notez également que l'adresse link-local de l'interface série 0/0/0 du routeur R1 est identique à celle de l'interface GigabitEthernet 0/0. Les interfaces série n'ont pas d'adresse MAC Ethernet. Cisco IOS utilise donc l'adresse MAC de la première interface Ethernet disponible. Cela est possible, car les interfaces link-local ne doivent être uniques que sur une liaison.


### Itininéraire

#### Itinéraire statique
```
ip route ipDestination SonMasque ipProchainRouteur ‘DistanceAdministrative’
```

#### SSH et Telnet

Sécuriser accès Telnet/SSH sur routeur
```
Router(config-line)# line vty 0 4

Router(config-line)# password mot_de_passe

Router(config-line)# login

Router(config-line)# transport input {ssh | telnet}
```

SSH
```
R1(config)# hostname <nom> // nom du router

R1(config)# ip domain-name cisco.com // défini le nom de domaine

R1(config)# crypto key generate rsa // génère clés asymétriques 

R1(config)# username etudiant secret lpasur // défini un user local

R1(config)# line vty 0 4

R1(config-line)# transport input ssh // affecte communication SSH aux lignes VTY de 0 à 4

R1(config-line)# login local

R1(config-line)# exit

R1# show ip ssh // afficher le statut ssh sur le routeur ou le switch

R1# show ssh // afficher l’état des connexions ssh
```
Se connecter en ssh avec putty ou TeraTerm
Vous devez veiller à choisir l’option SSH et à utiliser le port TCP 22.

### Config interface routeur
```
Router(config)# interface type-and-number

Router(config-if)# description description-text

Router(config-if)# ip address ipv4-address subnet-mask

Router(config-if)# ipv6 address ipv6-address/prefix-length

Router(config-if)# no shutdown
```

### Exemple de config sécurisé

```
R1(config)# service password-encryption // chiffre mdp en clair

R1(config)# security passwords min-length 8 // longueur min pour les mdp

R1(config)# login block-for 120 attempts 3 within 60 // limite tentatives de connexions pendant 120 secondes après 3 échecs en moins de 60 secondes

R1(config)# line vty 0 4 

R1(config-line)# password cisco123 

R1(config-line)# exec-timeout 5 30 // déconnexion si inactif pendant 5min30

R1(config-line)# transport input ssh 

R1(config-line)# end 

R1# show running-config | section line vty

line vty 0 4

 password 7 094F471A1A0A

 exec-timeout 5 30

 login

 transport input ssh

R1#

R1(config)# ip domain name span.com // Configurez le nom de domaine IP du réseau 

R1(config)# crypto key generate rsa general-keys modulus 1024 // génère clé d’authentification pour crypter le trafic

The name for the keys will be: Rl.span.com % The key modulus size is 1024 bits

% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

Dec 13 16:19:12.079: %SSH-5-ENABLED: SSH 1.99 has been enabled

R1(config)#

R1(config)# username Bob secret cisco // Créez une entrée de nom d'utilisateur dans la base de données locale

R1(config)# line vty 0 4

R1(config-line)# login local // authentifier la ligne vty par rapport à la base de données locale

R1(config-line)# transport input ssh // Activez les sessions SSH entrantes de vty

R1(config-line)# exit

R1# show ip ports all // vérifier les services actif par défaut

R1(config)# no ip http server // désactive HTTP

R1(config)# line vty 0 15

R1(config-line)# transport input ssh // désactive Telnet en spécifiant uniquement SSH
```

### Configuration LLA (adresse link-local monodiffusion) statique sur un routeur

![](https://lh5.googleusercontent.com/GSq0H39TdwFtYNVu0x_xxlFq0orFE8Q0ainlMXd7Rn_lR156_kZw3OEmO7gE6WPuLOPKlzbD6s7EQcyNh2poTNa0CqIGCpgGd58JT379fBmjKG8gVBYwnX-LbV6r4HyJ14N2ZFRNC-GgP3LREzFTjtlIEXt7umbQV3YPXbdIWFdMaeBbolTe6esa-qMvcQ)

```
R1(config)# interface gigabitethernet 0/0/0

R1(config-if)# ipv6 address fe80::1:1 link-local

R1(config-if)# exit

R1(config)# interface gigabitethernet 0/0/1

R1(config-if)# ipv6 address fe80::2:1 link-local

R1(config-if)# exit

R1(config)# interface serial 0/1/0

R1(config-if)# ipv6 address fe80::3:1 link-local

R1(config-if)# exit

R1(conf)# ipv6 unicast-routing // Active routage IPv6
```

### Accès à distance pour switch

Pour se connecter à un commutateur et le gérer sur un réseau IP local, une interface virtuelle de commutateur (SVI) doit être configurée. Le SVI est configuré avec une adresse IPv4 et un masque de sous-réseau sur le réseau local. Le commutateur doit également avoir une adresse de passerelle par défaut configurée pour gérer le commutateur à distance à partir d'un autre réseau.

```
Switch(config)#interface vlan1

Switch(config-if)#ip address 192.168.1.2 255.255.255.0

Switch(config-if)#no shutdown // active

Switch(config-if)#exit
```

Configurer une passerelle par défaut sur un commutateur. L'adresse IPv4 configurée est celle de l'interface de routeur du commutateur connecté.
```
S1# ip default-gateway adresseIP
```

### ACL

>Les ACL sont appliquées dans l’ordre par la machine, la première ACL (en haut) sera la première. Donc l’ordre est très important.

```
Router(config)# access-list 2 deny <ip de l’extérieur> <ip intérieur>

Router(config)# access-list 2 deny 192.168.10.1 0.0.0.0 // refuse l’ip 192.168.10.1 avec n’importe quel masque

Router(config)# access-list 2 permit 192.168.10.0 0.0.0.255 // autorise les ip du réseau 192.168.10.0 avec un masque de 255.255.255.0

Router(config)# access-list 2 deny 192.168.0.0 0.0.255.255 // refuse les ip du réseau 192.168.0.0 avec un masque de 255.255.0.0

Router(config)# access-list 2 permit 192.0.0.0 0.255.255.255 // autorise les ip du réseau 192.0.0.0 avec un masque de 255.255.255.0

Router(config)# no access-list 2 permit 192.0.0.0 0.255.255.255 // supprime l’acl

Router(config)# interface fastethernet0/0

Router(config-if)# ip access-group 2 in // applique les ACL sur les paquets entrant du port fastethernet0/0
```
>[!INFO]
>Les masques sont inversés dans les commandes.
>Access-list >= 100 pour les acl étendu.

#### ACL étendu

```
Router(config)# access‐list 101 deny ip any host 192.168.34.2 // machine 192.168.34.2 inaccessible depuis l’extérieur.

Router(config)# interface fastethernet0/0

Router(config-if)# ip access-group 101 in

Router(config)# access-list 100 permit tcp 10.1.1.0 0.0.0.255 10.1.2.0 0.0.0.255 eq 21 // “eq” veut dire égal, et le numéro de port 21 utilisé par le serveur FTP. On autorise le réseau 10.1.1.0/24 à communiquer sur le port 21 (pour le FTP) avec le réseau 10.1.2.0/24.

Router(config)# access-list 100 permit tcp 10.1.1.0 0.0.0.255 10.1.2.0 0.0.0.255 neq 21 // “neq” veut dire non égal. Donc pareil mais on autorise le réseau 10.1.1.0/24 à communiquer sur tout sauf le port 21
```

Afficher les ACL
```
Router(config)# show access‐lists
```
Afficher les ACL avec leur interfaces
```
Router(config)# show running-config
```

#### Ports

```
Router(config)# access-list 102 permit tcp 192.168.1.5 any eq 80 // autorise le poste 192.168.1.5 à accéder au port 80 de l’extérieur 

Router(config)# access-list 102 permit tcp 192.168.1.5 any eq 21 // autorise le poste 192.168.1.5 à accéder au port 21 de l’extérieur 

Router(config)# access-list 102 deny tcp 192.168.1.5 any // refuse le poste 192.168.1.5 à l'accès à tous les ports. 

Router(config)# access-list 102 permit ip any any // accepte toutes les ip
```


### VLAN

Créer VLAN
```
Switch_A#conf t

Switch_A(config)#vlan 2

Switch_A(config-vlan)#name VLAN2

Switch_A(config-vlan)#exit
```

Supprimer VLAN
```
Switch_A#configure terminal

Switch_A(config)#no vlan 3

Switch_A(config)#exit
```

Afficher
```
Switch_A#show vlan

Switch_A#show vlan id 2

Switch_A#show vlan name VLAN2

Switch#show interfaces fastEthernet0/2 switchport
```

Affectation Ports
```
Switch_A#conf t

Switch_A(config)#interface range fastethernet 0/4 – 6 // de la fa 4 à 6

Switch_A(config-if)#switchport mode access

Switch_A(config-if)#switchport access vlan 2

Switch_A(config-if)#end
```

VLAN distribués sur plusieurs switch (TRUNK)
```
Switch_A(config)#interface fastethernet 0/1

Switch_A(config-if)#switchport mode trunk

Switch_A(config-if)#end // à faire sur les 2 commutateur, sur le port sur lequel ils communiquent
```

Faire des sous interfaces pour chaque VLAN
```
interface gigabitethernet 0/0.10
```

Actuver l'encapsulation
```
encapsulation dot1Q 10
```

ip helper-address 10.1.1.1  

mettre passerelle pour le serveur DHCP si on veut qu’il puisse distribuer des adresses en dehors de son réseau


#### VTP (VLAN Trunking Protocol)

Mettre le commutateur 1 en mode server
```
Switch_1(config)#vtp server

Switch_1(config)#vtp domain lpasur

Switch_1(config)#exit
```

Mettre le commutateur 2 en mode client
```
Switch_1(config)#vtp client

Switch_1(config)#vtp domain lpasur

Switch_1(config)#exit
```

Impossible de créer un VLAN depuis le client.
Les VLANs créés depuis le switch1(server) ont aussi été créés dans le switch2(client).
En revanche l’affectation des ports sur le switch1 ne sont pas reproduites sur le switch2.

### Spanning Tree Protocol (STP)

Afficher
```
Switch_1#show spanning-tree
```

Changer le pont racine
```
Switch_B(config)#spanning-tree vlan 1 priority 4096

Switch_B(config)#exit

Switch_B(config)#no spanning-tree vlan 1 priority 4096 // supprime
```


### NAT

Lorsque l'on a une seul adresse public pour plusieurs adresses privé

#### NAT statique

```
Router(config)# show ip nat translation // affiche les nat

Router(config)# show ip nat statistics // affiche les stat

Router(config)#interface gigabitEthernet0/0

Router(config-if)#ip nat inside // tague l’interface en entrée

Router(config-if)#exit

Router(config)#interface gigabitEthernet0/1

Router(config-if)#ip nat outside // tague l’interface en sortie

Router(config-if)#

Router(config-if)#exit

Router(config)#ip nat inside source static 192.168.1.10 100.100.100.4 // l’IP 192.168.1.10 sera traduite en 100.100.100.4 lors du passage par le routeur

ip source : 100.100.100.4

ip destination : 192.168.1.10

Router(config)#no ip nat inside source static 192.168.1.10 100.100.100.4 // supprimer le nat statique
```

#### NAT dynamique

```
Router(config)#ip nat pool public-access 100.100.100.2 100.100.100.10 netmask 255.255.255.240 // la plage d’adresse publique translaté va de 100.100.100.2 à 100.100.100.10

Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255 // acess-list pour définir les adresses locales qui pourront être translaté

Router(config)#ip nat inside source list 1 pool public-access //  déclare la plage d’adresse IP publique qui pourra être utilisée aléatoirement

Router(config)#interface gigabitEthernet0/0

Router(config-if)#ip nat inside 

Router(config-if)#exit

Router(config)#interface gigabitEthernet0/1

Router(config-if)#ip nat outside

Router(config-if)#end

Router#write // met en place la conf
```

### PAT

Le PAT mappe plusieurs adresses privées vers une seule et même adresse publique en utilisant différents ports pour permettre de suivre la connexion. Nos BOX internet utilisent ça.

```
Routeur(config)# interface gigabitEthernet0/1

Routeur(config-if)# ip address 218.115.20.2 255.255.255.224

Routeur(config-if)# ip nat outside

Routeur(config-if)# exit

Routeur(config)# access-list 1 permit 192.168.1.0 0.0.0.255

Routeur(config)# ip nat inside source list 1 interface Gig0/1 overload // indique au routeur de translater notre access-list, à travers notre interface sortie
```

### Redirection de port

```
Router#conf t

Router(config)#ip nat inside source static tcp 192.168.1.2 80 132.78.241.30 80
```
- **Ip nat inside source static**: cela indique une redirection permanente
- **TCP**: le protocole HTTP utilise le protocole TCP
- **192.168.1.2**: on précise l’adresse IP du serveur WEB
- **80**: cela correspond au serveur port WEB du serveur
- **132.78.241.30**: on précise l’adresse IP publique
- **80**: on précise le port qui sera redirigé

Rediriger un serveur en HTTPS
```
Router(config)#ip nat inside source static tcp 192.168.1.2 443 132.78.241.30 443
```

Rediriger un serveur DNS
```
Router(config)#ip nat inside source static udp 192.168.1.2 53 132.78.241.30 53
```

Rediriger un serveur FTP
```
Router(config)#ip nat inside source static tcp 192.168.1.2 21 132.78.241.30 21
```

Rediriger un serveur TFTP
```
Router(config)#ip nat inside source static udp 192.168.1.2 69 132.78.241.30 69
```

Rediriger un serveur DHCP
```
Router(config)#ip nat inside source static udp 192.168.1.2 67 132.78.241.30 67
```

### DHCP 

Activer le DHCP sur les postes

```
campus(config)#ip dhcp pool <nom> // crée un groupe, puis place le routeur en mode spécialisé de configuration DHCP

campus(dhcp-config)#network 172.16.12.0 255.255.255.0 // plage d’adresses à octroyer

campus(dhcp-config)#default-router 172.16.12.1 // passerelle par défaut

campus(dhcp-config)#dns-server 172.16.1.2 // adresse du serveur DNS

campus(dhcp-config)#domain-name foo.com // nom de domaine

campus(config)#ip dhcp excluded-address 172.16.12.1 172.16.12.11 // exclure une adresse ou une série d’adresses lors de l’assignation

campus #show ip dhcp binding // visualisation
```

Pour désactiver le service
```
no service dhcp
```

### Monitoring (Switch)

```
Switch(config)#monitor session 1 source interface fa 0/1 both // copie aussi bien les paquets reçus que transmis (both - les deux). Si l'on souhaite prendre que les paquets reçus on précisera rx, pour ceux transmis ce sera tx.

Switch(config)#monitor session 1 source interface fa 0/1 - 13 // espionne plusieur ports

Switch(config)#monitor session 1 destination interface fa 0/2 // vers quel port de destination on souhaite dupliquer le trafic en provenance du port source. 
  
Switch(config)#no monitor session 1 // supprime
```

### MAC statique

```
Switch(config)#mac-address-table static 00e0.2917.1884 vlan 1 interface fastethernet 0/4 // configure l’adresse mac d’un poste sur une interface

N’importe quel poste peut être connecté sur ce port et on peut échanger des infos.

En revanche, le PC avec l’adresse mac statique du port ne pourra communiquer que sur ce port.

Switch(config)#no mac-address-table static 00e0.2917.1884 vlan 1 interface fastethernet 0/4 // supprime
```

### Sécurité des ports

```
ALSwitch(config-if)#switchport port-security maximum 1 // limite le nombre d’adresses MAC par port à 1
```
Le port est dédié au premier PC qui s’est branché dessus (le PC4). Donc un autre PC ne peut pas l’utiliser. Par contre, le PC4 peut utiliser n’importe quel autre port.

Afficher
```
ALSwitch(config-if)#show port security
```

### RIP

>Regarde le chemin le plus ==court== par le **nombre de routeur à passer**.
>Ne prend pas en compte la bande passante des liaisons.



### OSPF

>Regard le chemin le plus ==rapide== par la **bandes passante des liaisons**.

voir [Info OSPF](https://cisco.goffinet.org/ccna/ospf/messages-ospf/)

1) Définir IP sur interfaces
2) Mettre en place OSPF

##### Config OSPF
```
router(config)#router ospf 1  
router(config-router)#network ip_reseau_voisin1 masque_inversé area 0  
router(config-router)#network ip_reseau_voisin2 masque_inversé area 0  
router(config-router)#end
```

##### Config Loopback du routeur
```
Router0B(config)# interface loopback 0
Router0B(config-if)# ip address adresse_IP 255.255.255.255
```
- Mémoriser la conf OSPF
- Supprimer la conf OSPF
```
Router0B(config)# no router ospf 1
```
- Le réinjecter
```
router(config)#router ospf 1  
router(config-router)#network ip_reseau_voisin1 masque_inversé area 0  
router(config-router)#network ip_reseau_voisin2 masque_inversé area 0  
router(config-router)#end
```

##### Informations
Afficher les voisins d'un routeur
```
Router0B#show ip ospf neighbor
ou
Router0B#show ip ospf neighbor detail
```

Afficher métrique d'interface
```
Router0B#show ip ospf interface
```

Afficher toute les routes connu et leur Area
```
Router0B#show ip ospf database
```

Afficher informations sur les routes
```
Router0B#show ip route
```

Afficher ID du routeur
```
Router0B#show ip protocols
```

##### Redistribution de route par défaut

```
router(config)#router ospf 1
router(config-router)#default-information originate
```

##### Définir coût d'un lien

```
router(config-if)# bandwidth 64 // spécifie une bande passe en KBit/S 
ou
router(config-if)# ip ospf cost 1562
```

>Pour tenir compte de la métrique des liens gigabits, on doit modifier la base de calcul des coûts ospf. Par defaut, Reference bandwidth est 100 Mbps.
```
router(config)#router ospf 1
router(config-router)#Auto-cost reference bandwidth 1000
```

##### Reset d'un routeur

```
router(config)# config-register 0x2102
router(config)# end
router# write erase
router# reload
Save?[yes/no]: n
reload?[confirm]
```


### Commutateur de niveau 3

