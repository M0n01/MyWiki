

![[model_TCP_Protocol.PNG]]

#### DNS 

- Système de noms de domaine. Traduit les noms de domaine tels que cisco.com, en adresses IP.
> *Port 53*

#### DHCP 

##### DHCPv4

- Protocole de configuration dynamique des hôtes pour IPv4. Un serveur DHCPv4 affecte dynamiquement les informations d'adressage IPv4 aux clients DHCPv4 au démarrage et permet de réutiliser les adresses lorsqu'elles ne sont plus nécessaires.
> *Serveur : Port 67  |  Client : Port 68*

*Lorsque les serveurs DHCP ne sont pas opérationnels sur un réseau, les postes de travail reçoivent des adresses appartenant à la plage 169.254.0.0/16.*

##### DHCPv6

- Protocole de configuration dynamique des hôtes pour IPv6. DHCPv6 est similaire à DHCPv4. Un serveur DHCPv6 affecte dynamiquement les informations d'adressage IPv6 aux clients DHCPv6 au démarrage.
> *Serveur : Port 67  |  Client : Port 68*

- **==Sans état==** : *Indique au client d'utiliser les informations contenues dans le message RA pour l'adressage, mais des paramètres de configuration supplémentaires sont disponibles à partir d'un serveur DHCPv6.*

- **==Avec état==** : *Cette option est la plus proche de DHCPv4. Dans ce cas, le message RA indique au client d'obtenir toutes les informations d'adressage à partir d'un serveur DHCPv6 avec état, à l'exception de l'adresse de passerelle par défaut qui est l'adresse lien-local IPv6 source du RA.*

![[DHCPP.PNG]]

#### SLAAC

- Autoconfiguration des adresses apatrides. Méthode qui permet à un périphérique d'obtenir ses informations d'adressage IPv6 sans utiliser un serveur DHCPv6.
- La méthode SLAAC permet aux hôtes de créer leur propre adresse de monodiffusion globale IPv6 unique sans les services d'un serveur DHCPv6.
- Le SLAAC utilise les messages RA ICMPv6 pour fournir l'adressage et d'autres informations de configuration qui seraient normalement fournies par un serveur DHCP. Un hôte configure son adresse IPv6 en fonction des informations envoyées dans le RA. Les messages RA sont envoyés par un routeur IPv6 toutes les 200 secondes.
![[SLAAC.png]]

#### SMTP

- Protocole de Transfert de Courrier Simple. Permet aux clients d'envoyer du courrier électronique à un serveur de messagerie et aux serveurs d'envoyer du courrier électronique à d'autres serveurs.
> *Sans chiffrement : Port 25*
> *Chiffrement implicite : Port 465*
> *Chiffrement explicite : 587*

#### POP3

- Protocole de la Poste, version 3. Permet aux clients de récupérer le courrier électronique à partir d'un serveur de messagerie et de le télécharger dans l'application de messagerie locale du client.
> *Port 110 et 995 (SSL)*

#### IMAP

- Protocole d'Accès aux Messages Internet. Permet aux clients d'accéder au courrier électronique stocké sur un serveur de messagerie ainsi que de maintenir le courrier électronique sur le serveur.
> *IMAP2 et IMAP4 : 143 (TCP)*
> *IMAP3 : 220 (TCP)*
> *IMAPS (TLS implicite) : 993 (TCP)*

#### FTP

- Protocole de Transfert de Fichiers. Définit les règles qui permettent à un utilisateur sur un hôte d'accéder et de transférer des fichiers vers et depuis un autre hôte via un réseau. Le FTP est un protocole de livraison de fichiers fiable,connexion orienté et reconnu.
> *Port 21 (écoute) et Port 20 (données par défaut)*

#### SFTP

- Protocole de Transfert de Fichiers SSH. En tant qu'extension du protocole Secure Shell (SSH), le SFTP peut être utilisé pour établir une session de transfert de fichiers sécurisée dans laquelle le transfert de fichiers est crypté. SSH est une méthode de connexion à distance sécurisée qui est généralement utilisée pour accéder à la ligne de commande d'un périphérique.
> *Port 22*

#### TFTP

- Protocole de Transfert de Fichiers Trivial. Un protocole de transfert de fichiers simple et sans connexion avec une livraison de fichiers sans accusé de réception. Produit moins de surcharge que le protocole FTP.
> *Port 69*

#### HTTP

- Protocole de Transfert Hypertexte. Ensemble de règles permettant d'échanger du texte, des graphiques, des sons, des vidéos et autres fichiers multimédia sur le web.
> *Port 80*

##### HTTPS 

- HTTP Sécuisé. Forme sécurisée de HTTP qui crypte les données échangées sur le World Wide Web.
> *Port 443*

#### REST 

- Transfert de l'État de représentation. Service Web qui utilise des interfaces de programmation d'applications (API) et des requêtes HTTP pour créer des applications Web.

#### TCP

- Protocole de Contrôle de Transmission. Permet une communication fiable entre des processus fonctionnant sur des hôtes distincts et fournit des transmissions fiables et reconnues qui confirment le succès de la livraison.

#### UDP

- Protocole de Datagramme Utilisateur. Permet à un processus s'exécutant sur un hôte d'envoyer des paquets à un processus s'exécutant sur un autre hôte. Cependant, l'UDP ne confirme pas la réussite de la transmission des datagrammes.

#### IPv4

- Protocole Internet version 4. Reçoit des segments de message de la couche transport, emballe les messages en paquets et adresse les paquets pour une livraison de bout en bout sur un réseau. IPv4 utilise une adresse 32 bits.

#### IPv6

- IP version 6. Similaire à IPv4 mais utilise une adresse 128 bits.

#### NAT

- Traduction des Adresses de Réseau. Traduit les adresses IPv4 d'un réseau privé en adresses IPv4 publiques uniques au monde.

#### ICMPv4

- Protocole de message de contrôle Internet pour IPv4. Fournit un retour d'information d'un hôte de destination à un hôte source sur les erreurs de livraison de paquets.

#### ICMPv6

- ICMP pour IPv6. Fonctionnalité similaire à ICMPv4, mais elle est utilisée pour les paquets IPv6.

#### ICMPv6 ND

- Détection de voisin ICMPv6. Inclut quatre messages de protocole utilisés pour la résolution d'adresses et la détection d'adresses en double.

#### OSPF

- Ouvrez d'abord le chemin le plus court. Protocole de routage d'état de liaison qui utilise une conception hiérarchique basée sur des zones. Il s'agit d'un protocole de routage interne standard ouvert.

#### EIGR

- Protocole de routage amélioré des passerelles intérieures. Protocole de routage propriétaire de Cisco qui utilise une métrique composite basée sur la largeur de bande, le délai, la charge et la fiabilité.

#### BGP

- Protocole de passerelle frontalière. Un protocole de routage de passerelle extérieure standard ouvert utilisé entre les fournisseurs de services Internet (ISPs). Le protocole BGP est également utilisé entre les ISPs et leurs clients privés plus importants pour échanger des informations de routage.

#### ARP

- Protocole de résolution des adresses. Fournit un mappage d'adresse dynamique entre une adresse IPv4 et une adresse physique.

#### Ethernet 

- Définit les règles relatives aux normes de câblage et de signalisation de la couche d'accès au réseau.

#### WLAN

- Réseau local sans fil. Définit les règles de signalisation sans fil sur les fréquences radio 2,4 GHz et 5 GHz.

#### SSH

Protocole permettant d'établir une communication chiffrée, donc sécurisée (on parle parfois de _tunnel_), sur un réseau informatique (intranet ou Internet) entre une machine locale (le _client_) et une machine distante (le _serveur_). La sécurité du chiffrement peut être assurée par différentes méthodes, entre autre par mot de passe ou par un système de clés publique/privée (mieux sécurisé, on parle alors de [cryptographie asymétrique](https://fr.wikipedia.org/wiki/cryptographie%20asym%C3%A9trique "https://fr.wikipedia.org/wiki/cryptographie asymétrique")).

Lors d'une connexion ssh à un serveur, l'hôte sauvegarde la clé publique du serveur. Si elle vient à changer (dans le cas d'un Man In The Middle ou d'un changement sur le serveur) alors on sera prévenu de ce changement avant de se connecter.

>[!INFO]
>- La clé privé doit être bien garder, elle est secrète. Si quelqu'un obtient ma clé privé, il peut se faire passer pour moi.
>- Tout le monde peut avoir la clé publique. Elle ne sert qu'à chiffré des messages.
>- La clé publique peut être calculé avec la clé privé mais pas l'inverse.

**Déroulement de la communication :**

- Poste_A veut se connecter au Poste_B
- Poste_B génère un message aléatoire, le chiffre avec la clé publique du Poste_A et l'envoi
- Poste_A déchiffre le message avec sa clé privé et le renvoi au Poste_B
- Poste_B le compare avec le message qu'il avait généré avant de le chiffré
- Si c'est le même cela signifie que le Poste_A possède la bonne clé privé et que c'est donc bien lui. 

![[ssh.png|600]]

Pour le RSA prendre RSA 4096 car 2048 trop juste.

>[!Bonne Pratique]
Désactiver la connexion via SSH à root avec un mot de passe. Donc obligation de se connecter via une clé.


## Connexion à distance

### [[Protocoles#SSH|SSH]]

#### VNC

*Virtual Network Computing* (Informatique virtuelle en réseau), est un système de visualisation et de contrôle de l'environnement de bureau d'un ordinateur distant. Il permet au logiciel client VNC de transmettre les informations de saisie du clavier et de la souris à l'ordinateur distant, possédant un logiciel serveur VNC à travers un réseau informatique. Il utilise le protocole **RFB** qui utilise le port 5900 UDP/TCP (serveur VNC). Il existe une version open source [noVNC](https://novnc.com/info.html).

#### RDP

Le RDP (Remote Desktop Protocol) permet d'utiliser un ordinateur de bureau à distance. C'est le plus couramment utilisé. Le protocole RDP a été initialement publié par Microsoft. Il est disponible pour la plupart des systèmes d'exploitation Windows, mais il peut également être utilisé avec les systèmes d'exploitation Mac.

#### [[Protocoles#HTTP|HTTP (HTTPS)]]


## Haute dispo

### Protocole HSRP (Hot Standby Router Protocol)

Le protocole HRSP est un FHRP propriétaire de CISCO qui est conçu pour permettre le basculement transparent d'un périphérique IPv4 au premier saut. Le protocole HSRP offre une disponibilité de réseau élevée, par le biais d'une redondance de routage au premier saut pour les hôtes IPv4 des réseaux configurés avec une adresse de passerelle par défaut IPv4. HSRP est utilisé dans un groupe de routeurs pour sélectionner un périphérique actif et un périphérique en veille. Dans un groupe d'interfaces de périphérique, le périphérique actif est celui qui est utilisé pour le routage des paquets ; le périphérique en veille est celui qui prend le relais en cas de défaillance du périphérique actif ou lorsque certaines conditions prédéfinies sont réunies. La fonction du routeur en veille HSRP est de surveiller l'état de fonctionnement du groupe HSRP et de prendre rapidement la responsabilité du réacheminement des paquets lorsque le routeur actif est défaillant.