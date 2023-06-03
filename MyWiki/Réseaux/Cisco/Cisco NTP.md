
Voir [[NTP]]

Généralement, les paramètres de date et d'heure sur un routeur ou un commutateur peuvent être définis à l'aide de l'une des deux méthodes :

- **Configurer manuellement la date et l'heure**
```
R1# clock set 16:01:00 sept 25 2020
```

- **Configurer le protocole NTP (Network Time Protocol)**

La meilleure solution consiste à configurer le protocole NTP sur le réseau. Ce protocole permet aux routeurs du réseau de synchroniser leurs paramètres temporels avec un serveur NTP.

Tant que le protocole NTP n'est pas configuré sur le réseau, la commande **show clock** affiche l'heure actuelle sur l'horloge logicielle
```
R1# show clock detail
```

Configurer 209.165.200.225 comme serveur NTP
```
R1(config)# ntp server 209.165.200.225
R1(config)# end
R1# show clock detail
```

Vérifier que R1 est synchronisé avec le serveur NTP à 209.165.200.225
```
R1# show ntp associations
R1# show ntp status
```

Vérifier configuration
```
S1# show ntp associations
```



