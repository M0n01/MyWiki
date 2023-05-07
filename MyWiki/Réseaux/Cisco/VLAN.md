
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


##### Inter-VLAN routeur (Router-on-a-stick)
Faire des sous interfaces pour chaque VLAN
```
R1(config)# interface G0/0/1.10
R1(config-subif)# description Default Gateway for VLAN 10 
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip add 192.168.10.1 255.255.255.0
R1(config-subif)# exit
R1(config)# interface G0/0/1.20
R1(config-subif)# description Default Gateway for VLAN 20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip add 192.168.20.1 255.255.255.0
R1(config-subif)# exit
R1(config)# interface G0/0/1
R1(config-if)# description Trunk link to S1
R1(config-if)# no shut
R1(config-if)# end
```

ip helper-address 10.1.1.1  

Il faut mettre une passerelle pour le serveur DHCP si on veut qu’il puisse distribuer des adresses en dehors de son réseau

##### Inter-VLAN Switch niveau 3

1) Créer les VLAN

```
D1(config)# vlan 10
D1(config-vlan)# name LAN10
D1(config-vlan)# vlan 20
D1(config-vlan)# name LAN20
D1(config-vlan)# exit
D1(config)#
```

2) Créer les interfaces VLAN SVI
```
D1(config)# interface vlan 10
D1(config-if)# description Default Gateway SVI for 192.168.10.0/24
D1(config-if)# ip add 192.168.10.1 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
D1(config)# 
D1(config)# int vlan 20
D1(config-if)# description Default Gateway SVI for 192.168.20.0/24
D1(config-if)# ip add 192.168.20.1 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
D1(config)#
```

3) Configurer les ports d'accès

```
D1(config)# interface GigabiteThernet1/0/6
D1(config-if)# description Access port to PC1
D1(config-if)# switchport mode access
D1(config-if)# switchport access vlan 10
D1(config-if)# exit
D1(config)# 
D1(config)# interface GigabiteThernet1/0/18
D1(config-if)# description Access port to PC2
D1(config-if)# switchport mode access
D1(config-if)# switchport access vlan 20
D1(config-if)# exit
```

4) Activer Routage
```
D1(config)# ip routing
```

###### Configuration du routage

1) Configurer le port router (celui connecter à un router ou autre switch N3)
```
D1(config)# interface GigabitEthernet0/0/1
D1(config-if)# description routed Port Link to R1
D1(config-if)# no switchport
D1(config-if)# ip address 10.10.10.2 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
D1(config)#
```

2) Activer Routage
```
D1(config)# ip routing
```

3) Configurer le routage
```
D1(config)# router ospf 10
D1(config-router)# network 192.168.10.0 0.0.0.255 area 0
D1(config-router)# network 192.168.20.0 0.0.0.255 area 0
D1(config-router)# network 10.10.10.0 0.0.0.3 area 0
D1(config-router)# ^Z
D1#
```

##### Avoir des info
```
show vlan [brief]
show interfaces switchport
show interfaces trunk
show running-config
show interfaces switchport
show running-config interface
ipconfig
show ip interface brief
show interfaces
```

##### VLAN pour VoIP
```
S3(config)# vlan 150
S3(config-vlan)# name VOICE 
S3(config-vlan)# exit
S3(config)# interface fa0/18
S3(config-if)# switchport mode access 
S3(config-if)# mls qos trust cos
S3(config-if)# switchport voice vlan 150
S3(config-if)# end
S3#
```

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
