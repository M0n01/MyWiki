
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
DLS1(config)# interface range gigabitEthernet 0/1-2
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