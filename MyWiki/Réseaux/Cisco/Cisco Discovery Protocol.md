
**CDP (Cisco Discovery Protocol)** est un protocole propriétaire de couche 2 de Cisco qui permet de rassembler des informations sur les périphériques Cisco qui partagent la même liaison de données. CDP fonctionne indépendamment des supports et protocoles et s'exécute sur tous les périphériques Cisco, tels que routeurs, commutateurs et serveurs d'accès.

Il aide à prendre des décisions en matière de conception réseau, de dépannage et de modifications matérielles. Il peut également être utilisé comme outil de détection réseau pour analyser les données des périphériques voisins.

Pour les périphériques Cisco, le protocole CDP est activé par défaut. Pour des raisons de sécurité, il peut être préférable de désactiver le protocole CDP sur l'ensemble d'un périphérique réseau, ou par interface. Le protocole CDP permet aux pirates de rassembler des données précieuses sur la couche réseau, telles que les adresses IP, les versions IOS, ainsi que les types de périphériques.

Vérifier l'état du protocole CDP et afficher des informations sur ce dernier
```
Router# show cdp
```

Activer/désactiver CDP globalement pour toutes les interfaces prises en charge sur le périphérique
```
Router(config)# cdp run      // active
Router(config)# no cdp run   // désactive
```

Activer/désactiver CDP par interface
```
Switch(config)# interface gigabitethernet 0/0/1 
Switch(config-if)# cdp enable      // active
Switch(config-if)# no cdp enable   // désactive
```

Vérifier l'état du protocole CDP et afficher une liste des voisins
```
Router# show cdp neighbors
```

Afficher les interfaces qui sont compatibles CDP sur un périphérique
```
Router# show cdp interface
```

