
![[CAPTURE_TP3.png]]

Les PC sont déjà config

## Mise en place N2

### 1) Conf des switchs d'accès (les 3 du bas)

Activation de rapid-pvst
```
ALS1(config)# spanning-tree mode rapid-pvst
```

Création des 2 vlan (10 et 20)
```
ALS1(config)# vlan 10 
ALS1(config-vlan)# name VLAN10
```

Mise des interfaces dans les 2 vlan avec port-fast: 1-10:VLAN 10 et 11-20 VLAN20.
```
ALS1(config)# interface range fastEthernet 0/1-10 
ALS1(config-if)# switchport mode access 
ALS1(config-if)# switchport access vlan 10 
ALS1(config-if)# spanning-tree portfast
```

Mise en place des 2 liens trunk
```
ALS1(config)# interface range gigabitEthernet 0/1-2
ALS1(config-if)# switchport mode trunk
```

### 2) Conf des switchs de distribution (les 2 du milieu)

Activation de rapid-pvst
```
DLS1(config)# spanning-tree mode rapid-pvst
```

Création des 2 vlan (10 et 20)
```
DLS1(config)# vlan 10 
DLS1(config-vlan)# name VLAN10
```

Mise en place des liens trunk vers les switch d'acces et entre switch distributions
```
DLS1(config)# interface range fastEthernet 0/1-3
DLS1(config-if)# switchport trunk encapsulation dot1q 
DLS1(config-if)# switchport mode trunk

DLS1(config)# interface GigabitEthernet 0/1
DLS1(config-if)# switchport trunk encapsulation dot1q 
DLS1(config-if)# switchport mode trunk
```

Mise en place des priorités STP : 
switch distribution1 root VLAN 1 et 10 et secondaire 20 ; 
```
DLS1(config)# spanning-tree vlan 1 priority 4096 (ou root primary)
DLS1(config)# spanning-tree vlan 10 priority 4096 (ou root primary)
DLS1(config)# spanning-tree vlan 20 priority 8192 (ou root secondary)
```
switch distribution2 root VLAN20 et secondaire VLAN10 et 1;
```
DLS1(config)# spanning-tree vlan 20 root primary
DLS1(config)# spanning-tree vlan 10 root secondary
DLS1(config)# spanning-tree vlan 1 root secondary
```

>Faire test ping PC même VLAN

## Mise en place N3

### 1) Création des SVI sur les 2 switchs de distribution avec HSRP

adresse de Passerelle virtuelle 172.16.<10 ou 20 >.254 
adresse de switch de distribution réelle : 172.16.<10 ou 20 >.<251 ou 252> 

Distribution1 prioritaire pour le VLAN10 
```
SW_DISTRIB(config)# ip routing 

SW_DISTRIB(config)# interface vlan 10 
SW_DISTRIB(config-if)# ip address 172.16.10.251 255.255.255.0 
SW_DISTRIB(config-if)# exit 

SW_DISTRIB(config)# interface vlan 20 
SW_DISTRIB(config-if)# ip address 172.16.20.251 255.255.255.0 
SW_DISTRIB(config-if)# exit 

SW_DISTRIB(config)# interface vlan 10 
SW_DISTRIB(config-if)# standby version 2 
SW_DISTRIB(config-if)# standby 10 ip 172.16.10.254 
SW_DISTRIB(config-if)# standby 10 preempt 
SW_DISTRIB(config-if)# standby 10 priority 110
SW_DISTRIB(config-if)# exit

SW_DISTRIB(config)# interface vlan 20
SW_DISTRIB(config-if)# standby version 2 
SW_DISTRIB(config-if)# standby 10 ip 172.16.20.254 
SW_DISTRIB(config-if)# standby 10 preempt 
SW_DISTRIB(config-if)# standby 10 priority 90
SW_DISTRIB(config-if)# exit
```

Distribution2 prioritaire pour le VLAN20 
```
SW_DISTRIB(config)# ip routing 
SW_DISTRIB(config)# interface vlan 10 
SW_DISTRIB(config-if)# ip address 172.16.10.252 255.255.255.0 
SW_DISTRIB(config-if)# exit 

SW_DISTRIB(config)# interface vlan 20 
SW_DISTRIB(config-if)# ip address 172.16.20.252 255.255.255.0 
SW_DISTRIB(config-if)# exit 

SW_DISTRIB(config)# interface vlan 20 
SW_DISTRIB(config-if)# standby version 2 
SW_DISTRIB(config-if)# standby 10 ip 172.16.20.254 
SW_DISTRIB(config-if)# standby 10 preempt 
SW_DISTRIB(config-if)# standby 10 priority 110
SW_DISTRIB(config-if)# exit

SW_DISTRIB(config)# interface vlan 10
SW_DISTRIB(config-if)# standby version 2 
SW_DISTRIB(config-if)# standby 10 ip 172.16.10.254 
SW_DISTRIB(config-if)# standby 10 preempt 
SW_DISTRIB(config-if)# standby 10 priority 90
SW_DISTRIB(config-if)# exit
```

>Faire test Ping PC différent VLAN (vérifier les gateway)


### 2) Coeur de réseaux

Mettre des ip sur toute les interfaces

Core1
```
Switch(config)# ip routing
Switch(config)#interface gigabitEthernet 0/2
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.11.2 255.255.255.252
Switch(config-if)#no shutdown

Switch(config)#interface gigabitEthernet 0/1
Switch(config-if)#no switchport
Switch(config-if)#ip address 172.20.1.253 255.255.255.0
Switch(config-if)#no shutdown

Switch(config)#interface fastEthernet 0/11
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.12.2 255.255.255.252
Switch(config-if)#no shutdown

Switch(config)#interface fastEthernet 0/1
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.1.2 255.255.255.252
Switch(config-if)#no shutdown
```

Core2
```
Switch(config)# ip routing
Switch(config)#interface gigabitEthernet 0/2
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.22.2 255.255.255.252
Switch(config-if)#no shutdown

Switch(config)#interface gigabitEthernet 0/1
Switch(config-if)#no switchport
Switch(config-if)#ip address 172.20.1.252 255.255.255.0
Switch(config-if)#no shutdown

Switch(config)#interface fastEthernet 0/11
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.21.2 255.255.255.252
Switch(config-if)#no shutdown

Switch(config)#interface fastEthernet 0/1
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.2.2 255.255.255.252
Switch(config-if)#no shutdown
```

Distribution1
```
Switch(config)#interface gigabitEthernet 0/2
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.11.1 255.255.255.252
Switch(config-if)#no shutdown

Switch(config)#interface fastEthernet 0/11
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.21.1 255.255.255.252
Switch(config-if)#no shutdown
```

Distribution2
```
Switch(config)#interface gigabitEthernet 0/2
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.22.1 255.255.255.252
Switch(config-if)#no shutdown

Switch(config)#interface fastEthernet 0/11
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.12.1 255.255.255.252
Switch(config-if)#no shutdown
```

### 3) Routeurs

#### Routeur 0
```
Switch(config)# ip routing
Routeur(config)#interface FastEthernet 0/0
Routeur(config-if)#ip address 123.123.123.1 255.255.255.0
Routeur(config-if)#no shutdown

Routeur(config)#interface FastEthernet 0/1
Routeur(config-if)#ip address 192.168.1.1 255.255.255.252
Routeur(config-if)#no shutdown
```

Route par défaut et redistribution de cette route par OSPF
```
Routeur(config)#ip route 0.0.0.0 0.0.0.0 123.123.123.3

router(config)#router ospf 1
router(config-router)#default-information originate
```

PAT
```
Routeur(config)# interface fastEthernet 0/0
Routeur(config-if)# ip nat outside
Routeur(config-if)# exit
Routeur(config)# access-list 1 permit 172.16.10.0 0.0.0.255
Routeur(config)# access-list 1 permit 172.16.20.0 0.0.0.255
Routeur(config)# ip nat inside source list 1 interface Fast0/1 overload // indique au routeur de translater notre access-list, à travers notre interface sortie
```

#### Routeur 1
```
Switch(config)# ip routing
Routeur(config)#interface FastEthernet 0/0
Routeur(config-if)#ip address 123.123.123.2 255.255.255.0
Routeur(config-if)#no shutdown

Routeur(config)#interface FastEthernet 0/1
Routeur(config-if)#ip address 192.168.2.1 255.255.255.252
Routeur(config-if)#no shutdown
```

Route par défaut et redistribution de cette route par OSPF
```
Routeur(config)#ip route 0.0.0.0 0.0.0.0 123.123.123.3

router(config)#router ospf 1
router(config-router)#default-information originate
```

PAT
```
Routeur(config)# interface fastEthernet 0/0
Routeur(config-if)# ip nat outside
Routeur(config-if)# exit
Routeur(config)# access-list 1 permit 172.16.10.0 0.0.0.255
Routeur(config)# access-list 1 permit 172.16.20.0 0.0.0.255
Routeur(config)# ip nat inside source list 1 interface Fast0/1 overload // indique au routeur de translater notre access-list, à travers notre interface sortie
```

#### Routeur FAI
```
Switch(config)# ip routing
Routeur(config)#interface FastEthernet 0/0
Routeur(config-if)#ip address 123.123.123.3 255.255.255.0
Routeur(config-if)#no shutdown

Routeur(config)#interface FastEthernet 0/1
Routeur(config-if)#ip address 100.100.100.254 255.255.255.0
Routeur(config-if)#no shutdown
```