
> Voir [[MyWiki/OS/Windows/Commandes de base#Réseau|Réseau Windows]]

Désactive les diffusions dirigées
```
no ip directed-broadcasts
```

Info interface
```
show interface F0/1
```

Affiche la table d’adresse MAC
```
show mac address-table
```

Clear la table
```
clear mac address-table dynamic
```

Affiche la table ARP
```
show ip arp
```

Affiche la table de routage sur le routeur
```
show ip route
show ipv6 route
```

Affiche adresse des interfaces Ethernet
```
show ipv6 interface brief
```

>Notez que chaque interface possède deux adresses IPv6. La deuxième adresse de chaque interface est la GUA configurée. La première adresse, celle qui commence par FE80, est l'adresse de monodiffusion link-local de l'interface. Souvenez-vous que l'adresse link-local est automatiquement ajoutée à l'interface lorsqu'une adresse de diffusion globale est attribuée.

>Notez également que l'adresse link-local de l'interface série 0/0/0 du routeur R1 est identique à celle de l'interface GigabitEthernet 0/0. Les interfaces série n'ont pas d'adresse MAC Ethernet. Cisco IOS utilise donc l'adresse MAC de la première interface Ethernet disponible. Cela est possible, car les interfaces link-local ne doivent être uniques que sur une liaison.

