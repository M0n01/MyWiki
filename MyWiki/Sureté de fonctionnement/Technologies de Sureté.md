
## Load Balencing

==**Load balencer**== : Répartition de charge. Permet de faire de la Redondance. Il faut doubler le Load Balencer au cas où il y en ai un qui tombe.
- Robin Robin
- En fonction de la puissance CPU (1 à 50%, 1 à 25%...)
- En fonction du nombre de requête 

## VM

Snapshot != Backup

**==Accès par pont==** : Le mode d'accès réseau par pont de VirtualBox crée un pont entre la carte réseau d'une VM et celle du PC hôte. La VM peut ainsi appartenir au même réseau local que celui du PC hôte.

**==NAT (en réalité PAT)==** : Un routeur virtuel entre la carte réel et la carte virtuel. Le routeur virtuel fournit aussi une adresse IP (il fait office de DHCP).
Masquerade

**==Host Only==** : Une carte virtuelle sur l'hôte connecté avec la carte virtuelle de la VM. Pour communiquer entre une VM et la machine hôte. On peut communiquer avec le réseau si on active le routage sur la machine hôte.

>[!INFO]
**Exemple d'hyperviseur** : DirectX, VirtualBox, VMWare, Qemu

==**Mode promiscuité**== : Dans un pont cela permet de laisser tout le trafic passer et pas seulement celui qui nous est destiné. Pour écouter le trafic réseau.

## Conteneurs

Les conteneurs sont différents des VM.
Si on a plusieur VM qui ont le même OS (et le même que l'hôte), autant utiliser l'OS de la machine hôte, donc pas d'OS supplémentaire et pas d'hyperviseur. Pour les mêmes ressources, une machine qui fait tourner des serveurs web avec Dockeur peut en faire tourner 100 fois plus qu'une machine qui utilise des VM.
Dockeur est une surcouche de LXC.
*Pour lancer un conteneur il faut une image.*

**[Kubernetes](https://kubernetes.io/fr/)** : Permet de faire un cluster et donc de gérer plusieur noeuds (Dockeur n'en gère qu'1). Automatise les tâches opérationnelles de gestion des conteneurs et inclut des commandes intégrées permettant de déployer des applications, de leur apporter des modifications, d'effectuer un scaling à la hausse ou à la baisse en fonction des besoins, de surveiller les applications, et bien plus encore. La gestion des applications est ainsi facilitée.

*Dockeur ne possède pas d'interface graphique.*
**[Portainer](https://www.portainer.io/)** : Interface graphique pour Dockeur. Serveur web pour manager les conteneurs. C'est une interface web situé dans un conteneur. Il en existe d'autre.

Voir [[Protocoles#Connexion à distance| Connexion à distance]]

## RAID

==**RAID 0**== : Plusieurs disques qui ne font qu'un --> vitesse de lecture ecriture x4 (pour les HDD) mais même principe pour SSD.

==**RAID 1**== : Chaque données sur un disque est clonné sur un autre.

==**RAID 1 0**== : Des grappes (RAID 0) en double (RAID 1)

==**RAID 5**== : Un disque en "checksum" qui va additionner les valeurs des autres. Le problème c'est que le disque en checksum va recevoire bcp de lecture/ecriture. Donc on réparti l'espace de checksum sur les différents disques.
- Hot Spare
- Hot Plug


## Partitions

Bien pour séparer l'OS des données.

==**Partitions Statiques**== : Section d'un support de stockage

==**Partitions Dynamiques**== : 1 seule partition dans laquelle on fait des partitions virtuelles (LVM). C'est mieux que le partitionnement statique.

==**Hyperconvergence**== : un serveur qui gère un cluster de machine pour faire du RAID avec.

>[!REMARQUE]
Une machine sans disque, qui utilise un disque sur un serveur via un cable réseaux est plus rapide qu'un disque directement installé sur une machine

## Fail over service

1 machine qui tombe = 1 autre qui prend le relai

**[Keepalived](https://keepalived.readthedocs.io/en/latest/introduction.html)** : permet de faire basculer une adresse IP à un autre équipement si le premier tombe.

## Sauvegardes

**==Archive==** : Fichier qui contient des fichiers. Un iso est une archive.

- Transférer une archive contenant 1000 fichiers est beaucoup plus rapide que de transférer directement les 1000 fichiers. De plus, l'archive est bien moins lourde.

>[!REMARQUE]
Sur windows la compression de fichiers est pas ouf par rapport à Linux.

#### Protocoles de diques

- SATA/eSATA 6 Gbits/s
- SAS : 12 Gbits/s
Le probème des nappes est qu'à haut débit, il y a des interférences.