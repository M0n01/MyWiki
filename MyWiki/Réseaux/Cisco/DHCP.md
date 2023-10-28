
#### Serveur
Activer le DHCP sur les postes
```
campus(config)#ip dhcp pool <nom> // crée un groupe (un pool d'adresses), puis place le routeur en mode spécialisé de configuration DHCP

campus(dhcp-config)#network 172.16.12.0 255.255.255.0 // plage d’adresses à octroyer

campus(dhcp-config)#default-router 172.16.12.1 // passerelle par défaut

campus(dhcp-config)#dns-server 172.16.1.2 // adresse du serveur DNS

campus(dhcp-config)#domain-name foo.com // nom de domaine

campus(config)#ip dhcp excluded-address 172.16.12.1 172.16.12.11 // exclure une adresse ou une série d’adresses lors de l’assignation (ici de .1 à .11)

campus #show ip dhcp binding // visualisation
```

Pour désactiver le service
```
no service dhcp
```

Info
```
R1# show ip dhcp server statistics
```

#### Client
IP par DHCP sur une interface
```
SOHO(config)# interface G0/0/1 
SOHO(config-if)# ip address dhcp
SOHO(config-if)# no shutdown
```

#### DHCPv6

Voir [[Protocoles#DHCPv6|Protocole DHCPv6]]

Sans état
```
R1(config-if)# ipv6 nd other-config-flag
R1(config-if)# end
R1# 
R1# show ipv6 interface g0/0/1 | begin ND
```

Avec état
```
R1(config)# int g0/0/1
R1(config-if)# ipv6 nd managed-config-flag
R1(config-if)# ipv6 nd prefix default no-autoconfig
R1(config-if)# end
R1#
R1# show ipv6 interface g0/0/1 | begin ND
```

##### Server DHCPv6 sans état sur un Router

1) Activer le routage IPv6
```
R1(config)# ipv6 unicast-routing 
R1(config)#
```

2) Définir un nom de pool DHCPv6
```
R1(config)# ipv6 dhcp pool IPV6-STATELESS 
R1(config-dhcpv6)#
```

3) Configurer le pool DHCPv6
```
R1(config-dhcpv6)# dns-server 2001:db8:acad:1::254  // adresse serv DNS
R1(config-dhcpv6)# domain-name example.com 
R1(config-dhcpv6)# exit
R1(config)#
```

4) Lier le pool DHCPv6 à une interface
```
R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# description Link to LAN
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 nd other-config-flag
R1(config-if)# ipv6 dhcp server IPV6-STATELESS
R1(config-if)# no shut
R1(config-if)# end
R1#
```

5) Vérifier que les hôtes ont reçu les informations d'adressage IPv6
```
C:\PC1> ipconfig /all 

Configuration IP Windows 
Connection-specific DNS Suffix . : example.com 
DNS Servers . . . . . . . . . . . : 2001:db8:acad:1::1 

C:\PC1 >
```

##### Agent de relais DHCPv6

>Si le serveur DHCPv6 se trouve sur un réseau différent de celui du client, alors le routeur IPv6 peut être configuré en tant qu'agent de relais DHCPv6. La configuration d'un agent de relais DHCPv6 est similaire à celle d'un routeur IPv4 en tant que relais DHCPv4.

```
R1(config)# interface gigabitethernet 0/0/1  // interface coté client
R1(config-if)# ipv6 dhcp relay destination 2001:db8:acad:1::2 G0/0/0 R1(config-if)# exit
R1(config)#
```
- Cette commande est configurée sur l'interface face aux clients DHCPv6 et spécifie l'adresse du serveur DHCPv6 et l'interface de sortie pour atteindre le serveur, comme indiqué dans la sortie. L'interface de sortie n'est requise que lorsque l'adresse de saut suivant est un LLA.

