

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

### Afficher et sauvegarder la configuration

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


### Interface virtuelle switch (SVI) pour accès distant

>Pour accéder à distance au commutateur, une adresse IP et un masque de sous-réseau doivent être configurés sur l'interface SVI. 

Pour configurer une SVI sur un commutateur :
```
S1(config)# interface vlan 99
```
Vlan 99 n'est pas une interface physique réelle mais une interface virtuelle. 

Attribuez ensuite une adresse IPv4 : 
```
S1(config-if)# ip address adresseIP masque
```

Activez l'interface virtuelle : 
```
S1(config-if)# no shutdown
S1(config-if)# exit
```

Configurer la passerelle par défaut:
```
S1(config)# ip default-gateway adress_IP
S1(config)# end
```

### Itininéraire

#### Itinéraire statique
```
ip route ipDestination SonMasque ipProchainRouteur ‘DistanceAdministrative’
ip route 0.0.0.0 0.0.0.0 ipProchainRouteur  // route par défaut
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

Router(config-if)# no switchport

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


### Spanning Tree Protocol (STP)

>En couche 3 (IPv4 et IPv6) il y a le TTL pour éviter les boucles notamment. En couche 2 (Ethernet) on utilise STP.

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



### SLAAC

Voir [[Protocoles#SLAAC|Protocole SLAAC]]

>Bien que l'interface du routeur ait une configuration IPv6, elle n'est toujours pas activée pour envoyer des RA contenant des informations de configuration d'adresse aux hôtes utilisant le SLAAC.

Activer l'envoi de messages RA (rejoindre le groupe de routeurs IPv6)
```
R1(config)# ipv6 unicast-routing
R1(config)# exit
R1#
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

![[RIP.png]]

1) Mettre des IP aux interfaces des routeurs

2) Mettre des IP aux PC

3) Configuration RIP

(Router0/Router2)
```
router rip
version 2
network 192.168.2.0
network 172.16.0.0
no auto-summary
sh run
sh ip route
```

4) Configuration Passive Interface

Routeur0 :
```
router rip
version 2
passive-interface gigabitEthernet 0/0/0
```

Router2 :
```
passive-interface gigabitEthernet 0/0/0
```

5) Config Routeur1

```
router rip
network 192.168.1.0
network 192.168.3.0
network 172.16.40.0
no auto-summary
```

Sur Router0
```
network 192.168.1.0
Sur Router2
network 192.168.3.0
```


6) Conf réseau BLEU (zone static)

Sur Router 3 :
```
ip route 0.0.0.0 0.0.0.0 172.16.40.254
```

Sur Router1 :
```
ip route 172.16.10.0 255.255.255.0 172.16.40.253
redistribute static metric 5
```

7) Acces Internet

Sur Router1
```
ip route 0.0.0.0 0.0.0.0 100.100.100.2
router rip
default-information originate
```

8) Mise en place NAT/APT

Sur Router1 :
```
access-list 10 permit 172.16.0.0 0.0.255.255
interface gigabitEthernet 0/0/1
ip nat outside
interface gigabitEthernet 0/0/0
ip nat inside
interface serial 0/1/0
ip nat inside
interface serial 0/0/0
ip nat inside
ip nat inside source list interface GigabitEthernet 0/0/1 overload
```

9) DHCP

Sur Router0 :
```
ip dhcp pool LAN10
default-router 172.16.10.254
ip dhcp pool LAN20
default-router 172.16.20.254
ip dhcp pool LAN30
default-router 172.16.30.254
ip dhcp pool LAN40
default-router 172.16.40.254
ip dhcp excluded-address 172.16.10.250 172.16.10.254
interface loopback 0
router rip
network 10.10.10.1
```

Sur Router0 :
```
interface gigaethernet 0/0/0
ip helper-address 10.10.10.1
```

Sur Router2 :
```
interface gigaethernet 0/0/0
ip helper-address 10.10.10.1
```

Sur Router3 :
```
interface fastethernet 0/1
ip helper-address 10.10.10.1
```


### Dépannage

![[Dépannage_cisco.png]]

#### Pour routes statiques et par défaut

```
ping adresse_IP
traceroute adresse_IP
show ip route
show ip interface brief
show cdp neighbors detail // fournit une liste des dispositifs Cisco directement connectés. Cette commande valide la connectivité de couche 2 (et donc de couche 1). Par exemple, si un périphérique voisin est répertorié dans le résultat de la commande, mais qu'il ne peut pas être testé avec une commande ping, l'adressage de couche 3 doit être examiné.
```
