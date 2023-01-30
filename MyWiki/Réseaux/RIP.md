
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

8 - Mise en place NAT/APT

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

9 - DHCP

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

