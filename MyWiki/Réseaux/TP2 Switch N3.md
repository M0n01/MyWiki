
![[SwN3_Example.png]]

1) Mettre adresses IP et passerelle sur les postes
2) Faire VLAN 10 et 20 sur Switch N2
```
Switch#conf t
Switch(config)#vlan 10
Switch(config-vlan)#name VLAN10
Switch(config-vlan)#exit

Switch(config)#vlan 20
Switch(config-vlan)#name VLAN20
Switch(config-vlan)#exit
```

3) Affecter les ports au VLAN
```
Switch#conf t
Switch(config)#interface fastethernet 0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#end

Switch#conf t
Switch(config)#interface fastethernet 0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#end
```

4) Config l'Etherchannel sur le Switch N2
```
Switch(config)# interface gigabitethernet 0/1
Switch(config-if)# channel-group 2 mode active 
Switch(config)# interface gigabitethernet 0/2 
Switch(config-if)# channel-group 2 mode active 
Switch(config)# interface port-channel 2 
Switch(config-if)# switchport mode trunk 
Switch(config-if)# switchport trunk native VLAN 1 // Si paquet appartenant à aucun VLAN alors envoyé dans VLAN 1
Switch(config-if)# switchport trunk allowed VLAN 10,20,1
```

5) Activer le routage entre les VLAN sur le Switch N3
```
Switch_N3(config)#ip routing
```

6) Créer les VLAN sur le Switch N3
```
Switch_N3#conf t
Switch_N3(config)#vlan 10
Switch_N3(config-vlan)#name VLAN10
Switch_N3(config-vlan)#exit

Switch_N3(config)#vlan 20
Switch_N3(config-vlan)#name VLAN20
Switch_N3(config-vlan)#exit
```

7) Config l'EtherChannel sur le Switch N3
```
Switch_N3(config)# interface range gigabitEthernet 0/1 - 2 
Switch_N3(config-if-range)# switchport trunk encapsulation dot1q 
Switch_N3(config-if-range)# switchport mode trunk 
Switch_N3(config-if-range)# channel-group 2 mode active 
Switch_N3(config)#interface Port-channel 2 
Switch_N3(config-if)#switchport trunk encapsulation dot1q 
Switch_N3(config-if)#switchport mode trunk
```

Info Port channel et EtherChannel
```
Switch_N3#show interface port-channel 2 
Switch_N3#show etherchannel summary
```


8) Config interface SVI
```
Switch_N3(config)# interface vlan 10
Switch_N3(config-if)# ip address 192.168.10.254

Switch_N3(config)# interface vlan 20
Switch_N3(config-if)# ip address 192.168.20.254
```

9) Routeur

VIDE

## Partie 2

![[SwN3_Example2.png]]

1) Etherchannel N3 sur Switch 1 (sur les 2 switch et en changeant l'IP)
```
DLS1(config)# interface range fastEthernet 0/11 – 12 
DLS1(config-if-range)# no switchport 
DLS1(config-if-range)# channel-group 3 mode active

DLS1(config)# interface port-channel 3 
DLS1(config-if)# ip address 172.19.1.1 255.255.255.252
```
