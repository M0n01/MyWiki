Les VPN modernes prennent en charge les fonctionnalités de chiffrement, telles que les protocoles IPsec (Internet Protocol Security) et SSL (Secure Sockets Layer) pour sécuriser le trafic réseau entre sites.

**==Principaux avantages des VPN :==**

|                                                           |                                                                                                                                                                                                                                                                                  |
|:--------------------------------------------------------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **<span style="color:green">Réductions des coûts</span>** | Avec l'avènement de technologies économiques à large bande passante, les organisations peuvent utiliser des VPN pour réduire leurs coûts de connectivité en augmentant simultanément la bande passante de la connexion à distance                                                |
| **<span style="color:green">Sécurité</span>**             | Les VPN offrent le meilleur niveau de sécurité possible, en utilisant des protocoles de chiffrement et d'authentification avancés qui protègent les données d'un accès non autorisé                                                                                              |
| **<span style="color:green">Compatibilité</span>**        | Les VPN peuvent être mis en œuvre à partir d'une variété d'options de liaison WAN, y compris toutes les technologies à large bande populaires. Travailleurs distants peuvent profitez de ces connexions à haut débit pour obtenir un accès sécurisé à leurs réseaux d'entreprise |
| **<span style="color:green">Évolutivité</span>**          | Les VPN permettent aux organisations d'utiliser Internet, ce qui permet d'ajouter facilement de nouveaux utilisateurs sans ajouter d'infrastructure significative                                                                                                                |





## VPN site à site

Les VPN de site à site sont utilisés pour connecter des réseaux sur un réseau non fiable tel qu'Internet. Dans un VPN de site à site, les hôtes finaux envoient et reçoivent le trafic TCP / IP non chiffré normal par un périphérique de terminaison VPN. Le VPN est généralement appelé une passerelle VPN. Un VPN peut être un routeur ou un pare-feu, comme illustré dans la figure. Par exemple, l'appliance de sécurité adaptable Cisco (ASA) illustrée sur le côté droit de la figure est un périphérique de pare-feu autonome qui combine le pare-feu, le concentrateur VPN et la fonctionnalité de prévention des intrusions dans une image logicielle.

![[LEVPN.png]]

La passerelle VPN encapsule et chiffre le trafic sortant pour tout le trafic provenant d'un site particulier. Il envoie ensuite le trafic via un tunnel VPN sur Internet à une passerelle VPN sur le site cible. À la réception, la passerelle VPN de réception supprime les en-têtes, déchiffre le contenu et relaie le paquet vers l'hôte cible dans son réseau privé.

>[!Remarque]
>Les VPN de site à site sont généralement créés et sécurisés en utilisant la sécurité IP (IPSec).

## GRE sur IPSec

Le VPN IPSec standard (non GRE) ne peut créer que des tunnels sécurisés pour le trafic unicast. Par conséquent, les protocoles de routage n'échangeront pas d'informations de routage sur un VPN IPSec.

Pour résoudre ce problème, nous pouvons encapsuler le trafic du protocole de routage en utilisant un paquet GRE, puis encapsuler le paquet GRE dans un paquet IPSec pour le transmettre de manière sécurisée à la passerelle VPN de destination.

![[CLEVPN.png]]

**Protocole passager**
>Il est un paquet d'origine qui doit être encapsulé par GRE. Il peut être un paquet IPv4 ou IPv6, d'une mise à jour de routage, etc.

**Protocole de porteur**
>GRE est le protocole de transporteur qui encapsule le paquet passager d'origine.

**Protocole de transport**
>Il est un protocole qui sera réellement utilisé pour transmettre le paquet. Cela peut être IPv4 ou IPv6.

Par exemple, dans la figure affichant une topologie, la filiale et le Siège souhaitent échanger des informations de routage OSPF sur un VPN IPSec. Cependant, IPSec ne prend pas en charge le trafic de multidiffusion. Alors, GRE sur IPSec est utilisé pour prendre en charge le trafic du protocole de routage sur le VPN IPSec. Plus précisément, les paquets OSPF (c'est-à-dire le protocole passager) seraient encapsulés par GRE (c'est-à-dire le protocole porteur) et ensuite encapsulés dans un tunnel VPN IPsec.

![[GREEE.png]]

## VPN multipoint dynamique

Les VPN IPSec de site à site et GRE sur IPSec sont appropriés à utiliser lorsqu'il n'y a que quelques sites pour la connexion de manière sécurisée. Cependant, ils ne sont pas suffisants lorsque l'entreprise ajoute de nombreux autres sites. En effet, chaque site nécessiterait des configurations statiques pour tous les autres sites ou pour un site central.

DMVPN (Dynamic Multipoint VPN) est une solution logicielle de Cisco qui permet de créer plusieurs VPN de façon simple, dynamique et évolutive. Comme d'autres types de VPN, DMVPN s'appuie sur IPSec pour sécuriser le transport sur des réseaux publics, comme Internet.

## Interface de tunnel virtuel IPSec

Tel que DMVPN, l'interface de tunnel virtuel IPSec (VTI) simplifie le processus de configuration requis pour prendre en charge plusieurs sites et l'accès à distance. Les configurations IPSec VTI sont appliquées à une interface virtuelle au lieu de mapper de manière statique les sessions IPsec à une interface physique.

IPsec VTI est capable d'envoyer et de recevoir le trafic crypté IP unicast et multicast. Par conséquent, les protocoles de routage sont automatiquement pris en charge sans avoir besoin de configurer les tunnels GRE.

IPSec VTI peut être configuré entre les sites ou dans une topologie hub-and-spoke.

![[VTII.png]]


## VPN d'accès distant

![[VPN_ac.png]]

Les VPN d'accès à distance peuvent être créés en utilisant IPSec ou SSL. Comme la figure indique, un utilisateur distant doit initier une connexion VPN d'accès à distance.

La figure indique deux façons dont un utilisateur distant peut établir une connexion VPN d'accès à distance: VPN sans client et VPN basé sur client.

![[VPPN.png]]

- **La Connexion VPN sans client**
>La connexion est sécurisée en utilisant une connexion SSL par navigateur Web. SSL est principalement utilisé pour protéger le trafic HTTP (HTTPS) et les protocoles de messagerie tels que IMAP et POP3. Par exemple, HTTPS est en réalité HTTP qui utilise un tunnel SSL. La connexion SSL est d'abord établie, puis les données HTTP sont échangées via la connexion.

- **La Connexion VPN basée sur le client**
>Le logiciel client VPN tel que Cisco AnyConnect Secure Mobility Client doit être installé sur l'appareil de l'utilisateur distant. Les utilisateurs doivent commencer la connexion VPN en utilisant le client VPN, puis s'authentifier auprès de la passerelle VPN de destination. Une fois que les utilisateurs distants sont authentifiés, ils ont accès aux fichiers et aux applications d'entreprise. Le logiciel client VPN crypte le trafic en utilisant IPSec ou SSL et le transmet sur Internet à la passerelle VPN de destination.

### VPN SSL

Une fois qu'un client négocie une connexion VPN SSL avec la passerelle VPN, il se connecte réellement en utlisant le Transport Layer Security (TLS). TLS est la version la plus récente de SSL et est parfois exprimée en SSL / TLS. Cependant, les deux termes sont souvent utilisés de manière interchangeable.

SSL utilise l'infrastructure du clé publique et les certificats numériques pour authentifier les homologues. Les technologies VPN IPSec et SSL offrent un accès pratique à n'importe quelle application ou ressource réseau. 

*Cependant : *
>**IPSec** -> si la sécurité est un problème. 
>**SSL** -> si la prise en charge et la facilité de déploiement sont les principaux problèmes.

Le type de méthode VPN mis en œuvre est basé sur les exigences d'accès des utilisateurs et les processus informatiques de l'organisation. Le tableau compare les déploiements d'accès à distance IPsec et SSL.

![[VVVPPPPNN.png]]

IPSec et VPN SSL ne s'excluent pas mutuellement. Au contraire, ils sont complémentaires; les deux technologies résolvent des problèmes différents, et une organisation peut implémenter IPSec, SSL ou les deux, selon les besoins de ses télétravailleurs.


## VPN d'entreprise et de prestataire de service

De nombreuses options sont disponibles pour sécuriser le trafic d'entreprise. Ces solutions varient en fonction de la personne qui gère le VPN.

Les VPN peuvent être gérés et déployés comme:

- **VPN d'entreprise** 
>Les VPN gérés par l'entreprise sont des solutions similaires pour sécuriser le trafic d'entreprise sur l'internet. Les VPN de site à site et d'accès distant sont créés et gérés par l'entreprise à l'aide de VPN IPSec et SSL.

- **VPN de prestataire de service** 
>Les VPN gérés par le prestataire de service sont créés et gérés sur le réseau du prestataire. Le prestataire utilise la commutation d'étiquette multiprotocole (MPLS) au niveau de la couche 2 ou de la couche 3 pour créer des canaux sécurisés entre les sites d'une entreprise. MPLS est une technologie de routage que le prestataire utilise pour créer des chemins virtuels entre les sites. Cela sépare efficacement le trafic des autres trafics clients. Les autres solutions ancienne incluent le relais de trame et les VPN en mode de transfert asynchrone (ATM).

![[VPNNN.png]]

- **VPN MPLS de couche 3** 
>Le prestataire de service participe au routage client en établissant trunking entre les routeurs du client et les routeurs du prestataire. Les routes des clients qui sont reçues par le routeur du prestataire sont ensuite redistribués via le réseau MPLS vers les sites distants du client.

- **VPN MPLS de couche 2** 
>Le prestataire de services n'est pas impliqué dans le routage du client. Au lieu de cela, le prestataire déploie un service LAN privé virtuel (VPLS) pour émuler un segment LAN multi-accès Ethernet sur le réseau MPLS. Aucun routage n'est impliqué. Les routeurs au client appartiennent effectivement au même réseau à accès multiple.











