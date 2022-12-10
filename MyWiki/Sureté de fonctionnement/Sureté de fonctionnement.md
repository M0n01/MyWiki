**Mode promiscuité** : Dans un pont cela permet de laisser tout le trafic passer et pas seulement celui qui nous est destiné

**Fail over service** : 1 qui tombe = 1 autre qui prend le relai

## Load Balencing

**Load balencer** : Répartition de charge. Permet de faire de la Redondance. Il faut doubler le Load Balencer au cas où il y en a un qui tombe.
- Robin Robin
- En fonction de la puissance CPU (1 à 50%, 1 à 25%...)
- En fonction du nombre de requête 
**[Keepalived](https://keepalived.readthedocs.io/en/latest/introduction.html)** : permet de faire basculer une adresse IP à un autre équipement si le premier tombe.

## VM

Snapshot != Backup

Accès par pont :

NAT (en réalité PAT) : un routeur virtuel entre la carte réel et la carte virtuel. Le routeur virtuel fournit aussi une adresse IP (il fait office de DHCP)
Masquerade

Host Only : Une carte virtuelle sur l'hôte connecté avec la carte virtuelle de la VM. Pour communiquer entre une VM et la machine hôte. On peut communiquer avec le réseau si on active le routage sur la machine hôte.

**Hyperviseur** : [DirectX](https://fr.wikipedia.org/wiki/DirectX), VirtualBox, VMWare, Qemu

ProxMox

## Conteneurs

Les conteneurs sont différents des VM.
Si on a plusieur VM qui ont le même OS (et le même que l'hôte), autant utiliser l'OS de la machine hôte, donc pas d'OS supplémentaire et pas d'hyperviseur. Pour les mêmes ressources, une machine qui fait tourner des serveurs web avec Dockeur peut en faire tourner 100 fois plus qu'une machine qui utilise des VM.
Dockeur utilise LXC.
*Pour lancer un conteneur il faut une image.*

**[Kubernetes](https://kubernetes.io/fr/)** : Permet de faire un cluster et donc de gérer plusieur noeuds (Dockeur n'en gère qu'1). Automatise les tâches opérationnelles de gestion des conteneurs et inclut des commandes intégrées permettant de déployer des applications, de leur apporter des modifications, d'effectuer un scaling à la hausse ou à la baisse en fonction des besoins, de surveiller les applications, et bien plus encore. La gestion des applications est ainsi facilitée.

*Dockeur ne possède pas d'interface graphique.*
**[Portainer](https://www.portainer.io/)** : Interface graphique pour Dockeur. Serveur web pour manager les conteneurs. C'est une interface web situé dans un conteneur. Il en existe d'autre.

Voir [[RéseauxWiki#Connexion à distance| Connexion à distance]]

## Test Revoir

- Serveur ESX

- Cours Virtualisation
	- Emulation matérielle (GNS3)
	- Virtualisation totale (possible si processeur de même type)
	- Para virtualisation (le plus rapide)
	- Virtualisation au niveau matériel
	- Conteneurs

- OVA, ISO