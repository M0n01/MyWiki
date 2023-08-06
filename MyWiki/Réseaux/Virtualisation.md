
## Virtualisation de serveur

La virtualisation des serveurs permet de tirer parti des ressources inutilisées et de consolider le nombre de serveurs nécessaires. Ainsi, plusieurs systèmes d'exploitation peuvent coexister sur une plate-forme matérielle unique.

![[Hyperviseur.png]]

L'hyperviseur est un programme, un firmware ou un équipement matériel qui ajoute une couche d'abstraction au-dessus du matériel physique. La couche d'abstraction permet de créer des machines virtuelles qui ont accès à tous les composants matériels de la machine physique, notamment les processeurs, la mémoire, les contrôleurs de disque et les cartes réseau. Chaque machine virtuelle exécute un système d'exploitation complet et distinct.

## Avantages de la virtualisation

L'un des principaux avantages de la virtualisation est qu'elle permet de réduire le coût global :

- **Moins de matériel est nécessaire** - Permet de consolider les serveurs, ce qui nécessite moins de serveurs physiques, moins de dispositifs de réseau et moins d'infrastructures de support. Cela signifie également une réduction des coûts de maintenance.
- **Moins d'énergie est consommée** - Permet de réduire les coûts mensuels d'alimentation et de refroidissement. La baisse de consommation électrique aide les entreprises à réduire leur empreinte carbone.
- **Moins d'espace est nécessaire** - Réduit l'empreinte globale du centre de données. La diminution du nombre de serveurs, de périphériques réseau et de racks permet de réduire l'espace au sol requis.

La virtualisation présente également d'autres avantages :

- **Un prototypage plus facile** - Des laboratoires autonomes, fonctionnant sur des réseaux isolés, peuvent être rapidement créés pour tester et prototyper des déploiements de réseaux. En cas d'erreur, l'administrateur peut simplement revenir à une version précédente. Ces environnements de test peuvent être en ligne, mais ils sont isolés des utilisateurs finaux. Une fois les tests terminés, les serveurs et les systèmes peuvent être déployés auprès des utilisateurs.
- **Approvisionnement plus rapide des serveurs** - La création d'un serveur virtuel est bien plus rapide que l'approvisionnement d'un serveur physique.
- **Augmentation du temps de fonctionnement des serveurs** - La plupart des plateformes de virtualisation de serveurs offrent désormais des fonctions avancées de tolérance aux pannes redondantes, telles que la migration en direct, la migration du stockage, la haute disponibilité et la planification des ressources distribuées.
- **Amélioration de la reprise après désastre** - La virtualisation offre des solutions avancées de continuité des activités. Ainsi, grâce à l'abstraction matérielle, le site de reprise d'activité ne doit plus disposer d'équipements matériels identiques à ceux de l'environnement de production. La plupart des plates-formes de virtualisation de serveurs d'entreprise disposent également d'un logiciel qui permet de tester et d'automatiser le basculement avant qu'un désastre ne se produise.
- **Support des héritages** - La virtualisation peut prolonger la durée de vie des systèmes d'exploitation et des applications, ce qui laisse plus de temps aux entreprises pour migrer vers des solutions plus récentes.

## Hyperviseurs de type 2

Un hyperviseur de type 2 est un logiciel qui crée et exécute des instances de VM. L'ordinateur sur lequel un hyperviseur prend en charge une ou plusieurs machines virtuelles est appelé « machine hôte ». Les hyperviseurs de type 2 sont également appelés « hyperviseurs hébergés ». Cela est dû au fait que l'hyperviseur est installé sur le système d'exploitation existant, tel que macOS, Windows ou Linux. Une ou plusieurs instances du système d'exploitation sont ensuite installées au-dessus de l'hyperviseur, comme le montre la figure.

![[hyper2.png]]

Un avantage considérable des hyperviseurs de type 2 est qu'ils ne requièrent aucune console de gestion logicielle.

Les hyperviseurs de type 2 sont très populaires auprès des particuliers et des entreprises qui découvrent la virtualisation. Hyperviseurs de type 2 les plus courants :

- Virtual PC
- VMware Workstation
- Oracle VM VirtualBox
- VMware Fusion
- Mac OS X Parallels

## Hyperviseurs de type 1

Les hyperviseurs de type 1 sont également dits « sans système d'exploitation », car ils sont installés directement sur le matériel. Généralement, ils sont utilisés sur des serveurs d'entreprise et des périphériques de mise en réseau de centres de données.

Les hyperviseurs de type 1 bénéficient d'un accès direct aux ressources matérielles. Par conséquent, ils sont plus efficaces que les architectures hébergées. Ils améliorent l'évolutivité, les performances et la robustesse.

![[hyper1.png]]

## Installation d'une machine virtuelle sur un hyperviseur

Après l'installation d'un hyperviseur de type 1 et le redémarrage du serveur, seules des informations de base apparaissent, notamment la version du système d'exploitation, la quantité de mémoire vive et l'adresse IP. Il est impossible de créer une instance de système d'exploitation à partir de cet écran.

La gestion des hyperviseurs de type 1 nécessite en effet une « console de gestion ». Le logiciel de gestion permet de gérer plusieurs serveurs utilisant le même hyperviseur. La console de gestion peut consolider automatiquement les serveurs et allumer ou éteindre les serveurs selon les besoins.

Par exemple, supposons que le serveur 1 dans la figure devienne peu gourmand en ressources. Pour mettre plus de ressources à disposition, l'administrateur réseau utilise la console de gestion pour déplacer l'instance Windows vers l'hyperviseur sur Serveur2. La console de gestion peut également être programmée avec des seuils qui déclencheront automatiquement le déplacement.

![[hyper3.png]]

La console de gestion permet une reprise en cas de panne matérielle. Si un composant du serveur tombe en panne, la console de gestion déplace automatiquement la VM vers un autre serveur.

Certaines consoles de gestion permettent également la sur-allocation de serveurs. Par exemple, sur un serveur disposant de 16 Go de mémoire vive (RAM), un administrateur peut créer quatre instances de système d'exploitation et allouer à chacune 10 Go de RAM. Ce type de sur-allocation est utilisé couramment, dans la mesure où il est rare que les quatre instances fassent toutes appel simultanément aux 10 GB de RAM.

## Technologies de virtualisation des réseaux

Deux architectures réseau majeures ont été développées pour prendre en charge la virtualisation :

- **SDN (Software-Defined Networking)** - Une architecture de réseau qui virtualise le réseau, offrant une nouvelle approche de l'administration et de la gestion du réseau qui vise à simplifier et à rationaliser le processus d'administration.
- **ACI (Cisco Application Centric Infrastructure)** - Une solution matérielle spécialement conçue pour intégrer le cloud computing et la gestion des centres de données.

Les composantes du SDN peuvent comprendre les éléments suivants :

- **OpenFlow** - Cette approche a été développée à l'université de Stanford pour gérer le trafic entre les routeurs, les commutateurs, les points d'accès sans fil et un contrôleur. Le protocole OpenFlow constitue un élément fondamental dans la mise en œuvre des solutions SDN. 
- **OpenStack** - cette approche repose sur une plate-forme d'orchestration et de virtualisation pour créer des environnements cloud évolutifs et mettre en œuvre une solution IaaS (infrastructure en tant que service). OpenStack est souvent utilisé avec Cisco ACI. L'orchestration en réseau est le processus d'automatisation de l'approvisionnement des composants du réseau tels que les serveurs, le stockage, les commutateurs, les routeurs et les applications. 
- **Autres composants** - Parmi les autres composants figurent l'interface avec le système de routage (I2RS), l'interconnexion transparente de lots de liens (TRILL), le Cisco FabricPath (FP) et le Shortest Path Bridging (SPB) de la norme IEEE 802.1aq.

## Architectures traditionnelles et SDN

![[SDN.png]]

**Le contrôleur SDN est une entité logique qui permet aux administrateurs réseau de gérer et de définir la manière dont le plan de données des routeurs et des commutateurs virtuels doit gérer le trafic réseau. En d'autres termes, il orchestre, arbitre et simplifie la communication entre les applications et les composants réseau.**

![[SDN2.png]]

L'infrastructure SDN fait appel à des interfaces de programmation (API). Une API est un ensemble de requêtes normalisées qui définissent la façon dont une application soumet une requête de services à une autre application. Le contrôleur SDN utilise des API Northbound pour communiquer avec les applications en amont.

## Fonctionnement du contrôleur SDN

Le contrôleur SDN définit les flux de données entre le plan de contrôle centralisé et les plans de données sur des routeurs et des commutateurs individuels.

Pour pouvoir traverser le réseau, chaque flux doit être approuvé par le contrôleur SDN qui vérifie que la communication est autorisée dans le cadre de la politique réseau de l'entreprise. Si le contrôleur autorise le flux, il calcule l'itinéraire que ce dernier doit suivre et ajoute une entrée correspondante au flux dans tous les commutateurs situés sur le trajet.

Toutes les fonctions complexes sont prises en charge par le contrôleur. Le contrôleur alimente les tables de flux. Les commutateurs gèrent les tables de flux. Dans la figure, un contrôleur SDN communique avec les commutateurs prenant en charge OpenFlow à l'aide de ce protocole. Ce protocole s'appuie sur le protocole TLS (Transport Layer Security) pour sécuriser les communications issues du plan de contrôle à l'échelle du réseau. Chaque commutateur OpenFlow se connecte à d'autres commutateurs OpenFlow. Ils peuvent également se connecter aux périphériques utilisateur qui font partie d'un flux de paquets.

![[SDN1.png]]

Sur chaque commutateur, la gestion des flux de paquets est assurée par une série de tables mises en œuvre au niveau du matériel ou du firmware. À l'échelle du commutateur, un flux est une séquence de paquets qui correspond à une entrée spécifique dans une table de flux.

Les trois types de tableaux présentés dans la figure précédente sont les suivants :

- **Table des flux** - Ce tableau fait correspondre les paquets entrants à un flux particulier et spécifie les fonctions qui doivent être exécutées sur les paquets. Il peut y avoir plusieurs tables de flux qui fonctionnent à la manière d'un pipeline.
- **Table de groupe** - Un tableau de flux peut diriger un flux vers un tableau de groupe, ce qui peut déclencher diverses actions qui affectent un ou plusieurs flux
- **Table de comptage** - Cette table déclenche une série d'actions liées aux performances sur un débit, y compris la capacité de limiter le trafic.

## Types d'architecture SDN

#### SDN basé sur les périphériques

Dans ce type de SDN, les appareils sont programmables par des applications s'exécutant sur l'appareil lui-même ou sur un serveur du réseau, comme le montre la figure. Cisco OnePK est un exemple de SDN basé sur les périphériques. Il permet aux programmeurs de développer des applications en C et Java avec Python pour qu'elles s'intègrent et interagissent avec les périphériques Cisco.

![[SDNT1.png]]

#### SDN basé sur les contrôleurs

Ce type de SDN utilise un contrôleur centralisé qui connaît tous les appareils du réseau, comme le montre la figure. Les applications peuvent interagir avec le contrôleur, chargé de gérer les périphériques et de manipuler les flux de trafic sur le réseau. Le contrôleur Cisco Open SDN est une distribution commerciale d'OpenDaylight.

![[SDNT2.png]]

#### SDN basé sur les politiques

Ce type de SDN est similaire au SDN basé sur un contrôleur où un contrôleur centralisé a une vue sur tous les appareils du réseau, comme le montre la figure. Le SDN basé sur des politiques comprend une couche stratégique supplémentaire qui fonctionne à un niveau d'abstraction supérieur. Il utilise des applications intégrées qui automatisent les tâches de configuration avancée grâce à un workflow guidé et à une interface graphique utilisateur conviviale. Aucune compétence de programmation n'est nécessaire. Le contrôleur Cisco APIC-EM est un exemple de ce type de SDN.

![[SDNT3.png]]




