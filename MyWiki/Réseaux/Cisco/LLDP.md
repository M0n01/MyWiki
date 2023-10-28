
Le protocole LLDP (Link Layer Discovery Protocol) fait la même chose que [[Cisco Discovery Protocol|CDP]] mais il n'est pas spécifique aux périphériques Cisco.

LLDP fonctionne avec des périphériques réseau, tels que des routeurs, des commutateurs et des points d'accès LAN sans fil. Ce protocole annonce son identité et ses fonctionnalités aux autres périphériques et reçoit des informations d'un périphérique physiquement connecté de couche 2.

Activer/désactiver LLDP globalement sur un appareil réseau Cisco
```
Switch(config)# lldp run      // activer
Switch(config)# no lldp run   // désactiver
```

Tout comme CDP, LLDP peut être configuré sur les interfaces spécifiques. Cependant, LLDP doit être configuré séparément pour transmettre et recevoir des paquets LLDP
```
Switch(config)# lldp run
Switch(config)# interface gigabitethernet 0/1
Switch(config-if)# lldp transmit
Switch(config-if)# lldp receive
Switch(config-if)# end
```

Vérifier si le protocole LLDP a été activé sur le périphérique
```
Switch# show lldp
```

Découvrir les voisins
```
S1# show lldp neighbors
```

Obtenir la version IOS et l'adresse IP des voisins ainsi que la capacité de l'appareil
```
S1# show lldp neighbors detail
```

