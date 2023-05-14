
voir [[Théorie NAT et PAT]]

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
Routeur(config)# interface gigabitEthernet0/1  // interface de sortie

Routeur(config-if)# ip address 218.115.20.2 255.255.255.224
Routeur(config-if)# ip nat outside

Routeur(config-if)# exit

Routeur(config)# access-list 1 permit 192.168.1.0 0.0.0.255 // autoriser le réseau local

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
