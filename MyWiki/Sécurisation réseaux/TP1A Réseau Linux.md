## I ] Réalisation d'une infrastructure réseau privé avec DMZ simple

Créer une machine virtuelle avec une carte Ethernet en bridge sur le LAN IUT et une carte réseau en LAN privé LP. Créer une 2ème machine virtuelle avec une carte Ethernet en LAN interne LP

DESACTIVER PROXY SUR FIREFOX SI CA MARCHE PAS

### VM1 : ROUTEUR

Configurer les interfaces réseaux :
```bash
nano /etc/network/interfaces

auto eth1     # côté réseaux interne
iface eth1 inet static
address 192.168.0.1/24

auto eth0   # côté réseaux IUT
iface eth0 inet dhcp
```

Activer le routage entre les 2 cartes réseaux ( réseau IUT et LAN privé )
```bash
nano /etc/sysctl.conf

net.ipv4.ip_forward=1     # décommenter
```

Vérifier
```bash
sysctl -p
```


### Activer le PAT sur l'interface eth0 (Masquerade)  

nano /etc/network/interfaces

post-up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

  

service networking restart

  

### Configurer l’interfaces réseaux deuxième machine :

nano /etc/network/interfaces

auto eth0

iface eth0 inet static

address 192.168.0.2/24

gateway 192.168.0.1

  

nano /etc/resolv.conf

nameserver 192.168.2.1

nameserver 192.168.2.2

search rtprive.rt

  

service networking restart

  

## DMZ

  

### Configurer les interfaces réseaux(1er VM) : Ajouter carte réseau interne

nano /etc/network/interfaces

auto eth2

iface eth2 inet static

address 192.168.10.1/24

  

auto eth0

…

post-up iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.10.2:80

  

service networking restart

  

### Créer une nouvelle machine dans le réseaux DMZ et config la carte réseau puis installer un service http(3ème VM) :

nano /etc/network/interfaces

auto eth0

iface eth0 inet static

address 192.168.10.2/24

gateway 192.168.10.1

  

nano /etc/resolv.conf

nameserver 192.168.2.1

nameserver 192.168.2.2

search rtprive.rt

  

service networking restart

  

apt install apache2

  

Faire le test depuis la machine hôte : 

1.  Mettre IP VM1 côté LAN IUT dans les exceptions proxy
    
2.  Entrer IP VM1 dans navigateur
    
3.  Accéder au serveur apache
    

  
  
  
  
  
  
  
  

# II ] Réalisation d'une infrastructure réseau privée avec DMZ et liaison Internet

  

1.  ## Routage
    

  

### Création de la machine interne avec 3 carte réseaux dans lan10,lan20 et lan30 et configurations des interfaces réseaux :

nano /etc/network/interfaces

# côté réseaux interne

auto eth0

iface eth0 inet static

address 192.168.10.254/24

  

# côté réseaux DMZ

auto eth1

iface eth1 inet static

address 192.168.20.10/24

  

 # côté réseaux Relais

auto eth2 

iface eth2 inet static

address 192.168.30.10/24

gateway 192.168.30.254

  

### Activer le routage

nano /etc/sysctl.conf

net.ipv4.ip_forward=1     # décommenter

  

sysctl -p  # pour vérifier

  

service networking restart

  

### Création du client carte réseaux dans lan10 :

nano /etc/network/interfaces

auto eth0

iface eth0 inet static

address 192.168.10.1/24

gateway 192.168.10.254

  

nano /etc/resolv.conf

nameserver 192.168.2.1

nameserver 192.168.2.2

search rtprive.rt

### Création de la machine http (DMZ) carte réseaux dans lan20 et lan30 :

nano /etc/network/interfaces

auto eth0

iface eth0 inet static

address 192.168.20.100/24

  

auto eth1

iface eth1 inet static

address 192.168.30.100/24

gateway 192.168.30.254

  

nano /etc/resolv.conf

nameserver 192.168.2.1

nameserver 192.168.2.2

  

### Création de la machine relais avec 2 carte réseaux dans réseaux iut et lan30, configurations des interfaces réseaux :

nano /etc/network/interfaces

# côté réseaux iut

auto eth0

iface eth0 inet dhcp

  

# côté réseaux lan30

auto eth1

iface eth1 inet static

address 192.168.30.254/24

  

### Activer le routage :

nano /etc/sysctl.conf

net.ipv4.ip_forward=1 # décommenter

  

sysctl -p  # pour vérifier

  

service networking restart

  

### Activer le PAT sur l'interface eth0 (Masquerade)  

nano /etc/network/interfaces

post-up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

  

service networking restart

  

### Route statique vers réseaux interne :

sur relais : ip route add 192.168.10.0/24 via 192.168.30.10
(à refaire quand on redémarre VM)
sur machine dmz : ip route add 192.168.10.0/24 via 192.168.20.10
