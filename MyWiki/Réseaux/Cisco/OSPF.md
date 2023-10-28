
>Regarde le chemin le plus ==rapide== par la **bande passante des liaisons**.

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
>Dans "router ospf 1" : 1 = process-id (entre 1 et 65535). Le process-id doit être le même sur tous les routeurs de la zone OSPF

Définir ID du routeur
```
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1(config-router)# end
```

##### Config Loopback du routeur
```
Router0B(config)# interface loopback 0
Router0B(config-if)# ip address adresse_IP 255.255.255.255
```
>Si l'ID du routeur n'a pas été explicitement configuré ou appris précédemment, il utilisera l'adresse IPv4 de loopback (1.1.1.1 par exemple) comme ID du routeur.

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

Afficher toute les routes connu et leur Area (commun à toute les routeurs d'une même zone)
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

##### Config interface passive

```
R1(config)# router ospf 10 
R1(config-router)# passive-interface loopback 0
R1(config-router)# end
```
>Empêcher la transmission de messages de routage par une interface de routeur, tout en permettant à ce réseau d'être annoncé à d'autres routeurs.

##### Config du DR et BDR

- DR = Routeur désigné
- BDR = Routeur désigné de secours
```
R1(config)# interface GigabitEthernet 0/0/0 
R1(config-if)# ip ospf priority 255 
R1(config-if)# end
```
>Au lieu de se baser sur l'ID de routeur, il vaut mieux contrôler la sélection au moyen des priorités d'interfaces. Cela permet à un routeur d'être DR dans un réseau et DROther dans un autre. Dans "ip ospf priority valeur", la valeur est de 0 à 255. Plus la valeur est grande plus c'est probable que le routeur devienne le DR ou le BDR.

##### Config point à point

```
R1(config)# interface GigabitEthernet 0/0/0
R1(config-if)# ip ospf network point-to-point
```
>À utiliser sur toutes les interfaces sur lesquelles vous souhaitez désactiver le processus d'élection DR/BDR.

##### Effacer le processus OSPF

```
R1# clear ip ospf process
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

