
>La sécurité n'est aussi solide que le lien le plus faible du système, et la couche 2 est considérée comme ce lien faible. Cela est dû au fait que, les réseaux locaux étaient traditionnellement sous le contrôle administratif d'une seule organisation. Nous avons intrinsèquement fait confiance à toutes les personnes et tous les appareils connectés à notre réseau local. Aujourd'hui, avec le BYOD et des attaques plus sophistiquées, nos réseaux locaux sont devenus plus vulnérables à la pénétration. Par conséquent, en plus de protéger les couches 3 à 7, les professionnels de la sécurité réseau doivent également atténuer les attaques contre l'infrastructure LAN de couche 2.

## Bonnes pratiques

- Faire des VLAN privé


## ==Les Attaques du switch==

### Attaques de Table MAC

Toutes les tables MAC ont une taille fixe et par conséquent, un commutateur peut manquer de ressources pour stocker les adresses MAC. Les attaques par inondation d'adresses MAC profitent de cette limitation en bombardant le commutateur avec de fausses adresses sources MAC jusqu'à ce que la table d'adresses MAC du commutateur soit pleine.

Lorsque cela se produit, le commutateur traite la trame comme une monodiffusion inconnue et commence à inonder tout le trafic entrant sur tous les ports du même VLAN sans référencer la table MAC. Cette condition permet désormais à un acteur de menace de capturer toutes les trames envoyées d'un hôte à un autre sur le LAN local ou le VLAN local.

>[!Remarque]
>Le trafic n'est inondé que dans le LAN local ou le VLAN. L'acteur de menace ne peut capturer que le trafic au sein du LAN ou VLAN local auquel il est connecté.

>[!Info]
>Pour atténuer les attaques de débordement de la table d'adresses MAC, les administrateurs réseau doivent implémenter la [[Secu Réseaux#Sécurité des ports|sécurité des ports]].

#### Procédure d'attaque

1) L'acteur de menace est connecté au VLAN 10 et utilise **macof** pour générer rapidement de nombreuses adresses MAC et IP de source et de destination aléatoir.

2) Sur une courte période, la table MAC du commutateur se remplit.

3) Lorsque la table MAC est pleine, le commutateur commence à inonder toutes les trames qu'il reçoit. Tant que **macof** continue de fonctionner, la table MAC reste pleine et le commutateur continue d'inonder toutes les trames entrantes sur chaque port associé au VLAN 10.

4) L'acteur de menace utilise ensuite un logiciel de reniflage de paquets pour capturer les trames de tous les appareils connectés au VLAN 10.

>Si l'acteur de menace arrête l'exécution de **macof** ou est découvert et arrêté, le commutateur vieillit finalement les anciennes entrées d'adresse MAC de la table et recommence à agir comme un commutateur.

### Attaques de VLAN

>Comprend les attaques par saut et par double marquage de VLAN. Il comprend également les attaques entre les périphériques sur un VLAN commun.

#### Attaques sautantes de VLAN

Une attaque par saut de VLAN permet au trafic d'un VLAN d'être vu par un autre VLAN sans l'aide d'un routeur. Dans une attaque de saut de VLAN de base, l'acteur de menace configure un hôte pour qu'il agisse comme un commutateur afin de profiter de la fonction de port de trunking automatique activée par défaut sur la plupart des ports de commutateur.

L'acteur de menace configure l'hôte pour usurper la signalisation 802.1Q et le protocole DTP (Dynamic Trunking Protocol) propriétaire de Cisco pour signaler le trunk avec le commutateur de connexion. En cas de succès, le commutateur établit une liaison de trunk avec l'hôte, comme illustré dans la figure. L'acteur de menace peut désormais accéder à tous les VLAN sur le commutateur. L'acteur de menace peut envoyer et recevoir du trafic sur n'importe quel VLAN, sautant efficacement entre les VLAN.

![[AttaquesVLAN.PNG]]

#### Attaque de double marquage VLAN

Un acteur de menace dans des situations spécifiques pourrait intégrer une balise 802.1Q cachée dans le trame qui a déjà une balise 802.1Q. Cette balise permet au trame d'accéder à un VLAN que la balise 802.1Q d'origine n'a pas spécifié.

#### Procédure d'attaque

1) L'acteur de menace envoie un trame 802.1Q à double marquage au commutateur. L'en-tête externe a la balise VLAN de l'acteur de menace, qui est identique au VLAN natif du port de trunk. Pour les besoins de cet exemple, supposons qu'il s'agit du VLAN 10. La balise intérieure est le VLAN victime, dans cet exemple, le VLAN 20.
![[attaqueVLAN1.PNG]]

2) La trame arrive sur le premier commutateur, qui examine la première balise 802.1Q de 4 octets. Le commutateur voit que le trame est destinée au VLAN 10, qui est le VLAN natif. Le commutateur transfère le paquet sur tous les ports VLAN 10 après avoir supprimé la balise VLAN 10. La trame n'est pas repérée car elle fait partie du VLAN natif. À ce stade, la balise VLAN 20 est toujours intacte et n'a pas été inspectée par le premier commutateur.
![[AttaquesVLAN2.PNG]]

3) La trame arrive au deuxième commutateur qui n'a aucune connaissance qu'elle était censée être pour le VLAN 10. Le trafic VLAN natif n'est pas balisé par le commutateur d'envoi comme spécifié dans la spécification 802.1Q. Le deuxième commutateur ne regarde que la balise 802.1Q interne que l'acteur de menace a insérée et voit que le trame est destinée au VLAN 20, le VLAN cible. Le deuxième commutateur envoie le trame à la cible ou l'inonde, selon qu'il existe une entrée de table d'adresses MAC existante pour la cible.
![[AttaquesVLAN3.PNG]]

>[!Remarque]
>Une attaque de double marquage VLAN est unidirectionnelle et ne fonctionne que lorsque l'attaquant est connecté à un port résidant dans le même VLAN que le VLAN natif du port de trunk. L'idée est que le double marquage permet à l'attaquant d'envoyer des données à des hôtes ou des serveurs sur un VLAN qui autrement seraient bloqués par un certain type de configuration de contrôle d'accès. Vraisemblablement, le trafic de retour sera également autorisé, ce qui donnera à l'attaquant la possibilité de communiquer avec des périphériques sur le VLAN normalement bloqué.

En bref, une attaque par saut de VLAN peut être lancée de trois manières:

- Usurpation des messages DTP de l'hôte attaquant pour que le commutateur passe en mode de trunking. À partir de là, l'attaquant peut envoyer du trafic étiqueté avec le VLAN cible, et le commutateur délivre ensuite les paquets à la destination.
- Présentation d'un commutateur indésirable et activation de trunking. L'attaquant peut alors accéder à tous les VLAN sur le commutateur victime à partir du commutateur non autorisé.
- Un autre type d'attaque par saut de VLAN est une attaque à double étiquète (ou à double encapsulation). Cette attaque profite de la façon dont le matériel de la plupart des commutateurs fonctionne.

>[!Info]
>Voir la [[Secu Réseaux#Atténuation des attaques VLAN|méthode d'atténuation]].

### Attaques DHCP

#### Attaque de Famine DHCP

Le but de l'attaque de famine DHCP est de créer un DoS pour connecter les clients. Les attaques par épuisement des ressources DHCP reposent sur un outil d'attaque, **Gobbler**, par exemple.

**Gobbler** a la possibilité d'examiner l'intégralité des adresses IP louables et essaie de toutes les louer. Plus précisément, il crée des messages de découverte DHCP avec de fausses adresses MAC.

#### Attaque d'Usurpation DHCP

Une attaque par usurpation via le service DHCP se produit lorsqu'un serveur DHCP non autorisé (rogue) se connecte au réseau et fournit des paramètres de configuration IP incorrects aux clients légitimes. Un serveur malhonnête peut fournir toute une série d'informations trompeuses :

- **Passerelle par défaut incorrecte** - Le serveur escroc fournit une passerelle non valide ou l'adresse IP de son hôte pour créer une attaque d'homme au milieu. Cette approche peut passer totalement inaperçue, car l'intrus intercepte le flux de données via le réseau.
- **Serveur DNS incorrect** - Le serveur escroc fournit une adresse de serveur DNS incorrecte pointant l'utilisateur vers un site Web néfaste.
- **Adresse IP incorrecte** - TLe serveur escroc fournit une adresse IP invalide créant efficacement une attaque DoS sur le client DHCP.

##### Procédure d'attaque

1) **Un acteur de menace connecte un serveur DHCP non autorisé**
Un acteur de menace connecte avec succès un serveur DHCP non autorisé à un port de commutateur sur le même sous-réseau et les VLAN que les clients cibles. Le serveur non autorisé sert à fournir aux clients de fausses informations de configuration IP.

2) **Le client diffuse des messages de découverte DHCP**
Un client légitime se connecte au réseau et nécessite des paramètres de configuration IP. Par conséquent, le client diffuse une demande de découverte DHCP à la recherche d'une réponse d'un serveur DHCP. Les deux serveurs recevront le message et répondront.

4) **Réponse DHCP légitime et non autorisée**
Le serveur DHCP légitime répond avec des paramètres de configuration IP valides. Cependant, le serveur escroc répond également par une offre DHCP contenant des paramètres de configuration IP définis par l'acteur de menace. Le client répond alors à la première offre reçue.

6) **Le client accepte l'offre DHCP non autorisée**
L'offre d'escroquerie a été reçue en premier et, par conséquent, le client diffuse une demande DHCP acceptant les paramètres IP définis par l'acteur de menace. Les serveurs légitime et non autorisé ont reçu la requête.

8) **Serveur escroc confirme**
Le serveur escorc monodiffuse un message de réponse au client pour accuser réception de sa demande. Le serveur légitime cesse de communiquer avec le client.

>[!Info]
>Les deux attaques sont atténuées par l'implémentation de la [[Secu Réseaux#Surveillance DHCP|surveillance DHCP]].

### Attaques ARP

Comprend l'usurpation d'identité ARP et les attaques d'empoisonnement ARP.

Il existe de nombreux outils disponibles sur Internet pour créer des attaques ARP homme-au-milieu, notamment **dsniff**, **Cain & Abel**, **ettercap**, **Yersinia** et autres. IPv6 utilise le protocole de découverte de voisin ICMPv6 pour la résolution d'adresse de couche 2. IPv6 inclut des stratégies pour atténuer l'usurpation de publicité de voisin, de la même manière que IPv6 empêche une réponse ARP usurpée.

#### Procédure d'attaque

1) **État Normal avec Tables MAC Convergentes**
Chaque périphérique a une table MAC précise avec les adresses IP et MAC correctes pour les autres périphériques sur le LAN.

2) **Attaque d'Usurpation ARP**
L'acteur de menace envoie deux réponses ARP usurpées et gratuites pour tenter de remplacer R1 en tant que passerelle par défaut:
- Le premier informe tous les périphériques du LAN que l'adresse MAC de l'acteur de menace (CC: CC: CC) correspond à l'adresse IP de R1, 10.0.0.1.
- Le second informe tous les périphériques du LAN que l'adresse MAC de l'acteur de menace (CC: CC: CC) correspond à l'adresse IP du PC1, 10.0.0.11.

3) **Attaque Empoisonnante avec Attaque de L'Homme-au-Milieu ARP**
R1 et PC1 suppriment l'entrée correcte pour l'adresse MAC de l'un l'autre et la remplacent par l'adresse MAC de PC2. L'acteur de la menace a désormais empoisonné les caches ARP de tous les périphériques du sous-réseau. L'empoisonnement par ARP conduit à diverses attaques de l'homme au milieu, ce qui constitue une grave menace pour la sécurité du réseau.

![[ARPATTAQUE.PNG]]

>[!Info]
>L'usurpation ARP et l'empoisonnement ARP sont atténués par l'implémentation de [[Secu Réseaux#Inspection ARP dynamique (DAI)|DAI]].

### Attaques par usurpation d'adresse

Comprend les attaques d'usurpation d'adresse MAC et d'adresse IP.

Les attaques d'usurpation d'adresse MAC se produisent lorsque les acteurs de menace modifient l'adresse MAC de leur hôte pour correspondre à une autre adresse MAC connue d'un hôte cible. L'hôte attaquant envoie ensuite un trame dans l'ensemble du réseau avec l'adresse MAC nouvellement configurée. Lorsqu'un commutateur reçoit la trame, il examine l'adresse MAC source. Le commutateur écrase l'entrée de table MAC actuelle et attribue l'adresse MAC au nouveau port. Il transfère ensuite par inadvertance des trames destinées à l'hôte cible à l'hôte attaquant.

Lorsque l'hôte cible envoie du trafic, le commutateur corrige l'erreur, en réalignant l'adresse MAC sur le port d'origine. Pour empêcher le commutateur de ramener l'affectation de port à son état correct, l'acteur de menace peut créer un programme ou un script qui enverra constamment des trames au commutateur afin que le commutateur conserve les informations incorrectes ou usurpées. Il n'y a pas de mécanisme de sécurité au niveau de la couche 2 qui permet à un commutateur de vérifier la source des adresses MAC, ce qui le rend si vulnérable à l'usurpation d'identité

>[!Info]
>L'usurpation d'adresse IP et MAC peut être atténuée en implémentant [[Secu Réseaux#Protection de la source IP (IPSG)|IPSG]].

### Attaques STP

Comprend les attaques de manipulation du protocole Spanning Tree (arbre enjambant).

Les attaquants du réseau peuvent manipuler le protocole STP (Spanning Tree Protocol) pour mener une attaque en usurpant le pont racine et en modifiant la topologie d'un réseau. Les attaquants peuvent faire apparaître leurs hôtes comme des ponts racine; et par conséquent, capturez tout le trafic pour le domaine commuté immédiat.

Pour mener une attaque de manipulation STP, l'hôte attaquant diffuse des unités de données de protocole de pont STP (BPDU) contenant des modifications de configuration et de topologie qui forceront les recalculs de spanning-tree, comme illustré dans la figure. Les BPDU envoyés par l'hôte attaquant annoncent une priorité de pont inférieure pour tenter d'être élu pont racine.

![[STPATTA.PNG]]

En cas de succès, l'hôte attaquant devient le pont racine, comme le montre la figure, et peut désormais capturer une variété de trames qui autrement ne seraient pas accessibles.

![[STPPATTA.PNG]]

>[!Info]
>Cette attaque STP est atténuée par l'implémentation de BPDU Guard sur tous les ports d'accès. Voir [[Secu Réseaux#Atténuation attaques STP|atténuation attaques STP]]

### Reconnaissance CDP (Cisco uniquement)

Le protocole CDP (Cisco Discovery Protocol) est un protocole propriétaire de découverte de liaison de couche 2. Il est activé par défaut sur tous les périphériques Cisco. Le protocole CDP permet de détecter automatiquement d'autres périphériques CDP et ainsi d'aider à la configuration automatique de leur connexion. Les administrateurs réseau utilisent également le protocole CDP pour configurer et dépanner les périphériques réseau.

Cependant, les informations fournies par CDP peuvent également être utilisées par un acteur de menace pour découvrir les vulnérabilités de l'infrastructure du réseau.

Les diffusions CDP ne sont ni chiffrées, ni authentifiées. Par conséquent, un acteur de menace peut compromettre l'infrastructure de réseau en envoyant de fausses trames CDP contenant de fausses informations aux périphériques Cisco connectés.

>[!Info]
>Voir l'[[Secu Réseaux#Atténuation pour CDP|atténuation]].

## ==Techniques d'Atténuation des Attaques du switch==

### Atténuation des attaques VLAN

Les attaques de saut de VLAN et de double marquage VLAN peuvent être évitées en mettant en œuvre les directives de sécurité de jonction suivantes :

- Désactivez le trunking sur tous les ports d'accès.
- Désactivez le trunking automatique sur les liaisons de jonction afin que les trunks doivent être activées manuellement.
- Assurez-vous que le VLAN natif n'est utilisé que pour les liaisons de trunk.

Utilisez les étapes suivantes pour atténuer les attaques par saut de VLAN:

1) Désactivez les négociations DTP (jonction automatique) sur les ports sans trunk à l'aide de la commande de configuration de l'interface **switchport mode access** .

2) Désactivez les ports inutilisés et placez-les dans un VLAN inutilisé.

3) Activez manuellement la liaison de jonction sur un port de jonction à l'aide de la commande **switchport mode trunk** .

4) Désactivez les négociations DTP (trunking automatique) sur les ports de jonction à l'aide de la commande **switchport nonegotiate** .

5) Définissez le VLAN natif sur un VLAN autre que VLAN 1 à l'aide de la commande **switchport trunk native vlan** vlan_number.

### Sécurité des ports

Empêche de nombreux types d'attaques, y compris les attaques d'inondation d'adresses MAC et les attaques de famine DHCP. 

Il permet à un administrateur de configurer manuellement les adresses MAC d'un port ou de permettre au commutateur d'apprendre dynamiquement un nombre limité d'adresses MAC. Lorsqu'un port configuré avec la sécurité des ports reçoit une trame, le système recherche l'adresse MAC source de la trame dans la liste des adresses source sécurisées qui ont été configurées manuellement ou automatiquement (par apprentissage) sur le port.

### Surveillance DHCP

Empêche la famine du DHCP et les attaques d'usurpation du DHCP.

L'espionnage DHCP détermine si les messages DHCP proviennent d'une source de confiance ou non. Il filtre ensuite les messages DHCP et limite la fiabilité du trafic DHCP de sources qui ne sont pas approuvé.

Les périphériques sous contrôle administratif (par exemple, les commutateurs, les routeurs et les serveurs) sont des sources fiables. Tout appareil placé en dehors de votre réseau est une source non fiable. Par ailleurs, tous les ports d'accès sont généralement traités comme des sources non fiables.

Une table DHCP est créée qui inclut l'adresse MAC source d'un périphérique sur un port non approuvé et l'adresse IP attribuée par le serveur DHCP à ce périphérique. L'adresse MAC et l'adresse IP sont liées ensemble. Par conséquent, cette table est appelé table de liaison d'espionnage DHCP.

Utilisez les étapes suivantes pour activer l'espionnage DHCP (snooping):

1) Activez l'espionnage DHCP à l'aide de la commande de configuration globale **ip dhcp snooping** .

2) Sur les ports approuvés, configurez l'interface avec la commande **ip dhcp snooping trust**.

3) Limitez le nombre de messages de découverte DHCP pouvant être reçus par seconde sur les ports non approuvés à l'aide de la commande de configuration d'interface **ip dhcp snooping limit rate** .

4) Activez la surveillance DHCP par VLAN ou par une plage de VLAN à l'aide de la commande de configuration globale **ip dhcp snooping** _vlan_.

### Inspection ARP dynamique (DAI)

Empêche l'usurpation d'ARP et les attaques d'empoisonnement d'ARP.

Pour empêcher l'usurpation ARP et l'empoisonnement ARP qui en résulte, un commutateur doit garantir que seules les requêtes et les réponses ARP valides sont relayées.

L'inspection ARP Dynamique (DAI) nécessite l'espionnage DHCP (snooping) et aide à prévenir les attaques ARP en :

- Ne pas relayer les réponses ARP non valides ou gratuites vers d'autres ports du même VLAN.
- Interception de toutes les requêtes et les réponses ARP sur les ports non approuvés.
- Vérification de chaque paquet intercepté pour une liaison IP-MAC valide.
- Abandon et journalisation des réponses ARP provenant de non valides pour empêcher l'empoisonnement ARP.
- Error-disabling l'interface si le nombre DAI des paquets ARP configurés sont dépassées.

Pour atténuer les risques d'usurpation ARP et d'empoisonnement ARP, suivez ces directives d'implémentation DAI:

- Activez globalement L'espionnage DHCP (snooping ).
- Activer l'espionnage DHCP sur les VLAN sélectionnés.
- Activer l'inspection ARP dynamique (DAI) sur les VLAN sélectionnés.
- Configurer les interface approuvées avec l'espionnage DHCP et l'inspection ARP.

>[!Remarque]
>Il est généralement conseillé de configurer tous les ports de commutateur d'accès comme non approuvés et de configurer tous les ports de liaison montante qui sont connectés à d'autres commutateurs comme approuvés.

### Protection de la source IP (IPSG)

Empêche les attaques d'usurpation d'adresse MAC et IP.

### Atténuation attaques STP

Pour atténuer les attaques de manipulation du protocole Spanning Tree (STP), utilisez PortFast et Bridge Protocol Data Unit (BPDU) Garde :

- **PortFast** : Avec PortFast, une interface configurée comme un port d'accès ou trunc passe immédiatement de l'état de blocage à celui de transfert, et contourne ainsi les situations d'écoutes et d'apprentissages. Appliquer à tous les ports d'utilisateur final. PortFast ne doit pas être configuré que sur les ports connectés aux périphériques finaux.
- **BPDU Guard** : Une erreur de BPDU guard désactive immédiatement un port qui reçoit un BPDU. Comme PortFast, BPDU guard ne doit pas être configuré que sur les ports connectés aux périphériques finaux.

### Atténuation pour CDP

Pour réduire le risque d'attaque de CDP, limitez l'utilisation de ce protocole sur les périphériques et les ports. Par exemple, désactivez CDP sur les ports périphériques qui se connectent aux périphériques auxquels vous ne faites pas confiance.

Pour désactiver CDP globalement sur un périphérique, utilisez la commande du mode de configuration globale **no cdp run**. Pour activer CDP globalement, utilisez la commande de configuration globale **cdp run**.

Pour désactiver CDP sur un port, utilisez la commande de configuration d'interface **no cdp enable**. Pour activer CDP sur un port, utilisez la commande de configuration d'interface **cdp enable**.

>[!Remarque]
>Le protocole LLDP (Link Layer Discovery Protocol) est également vulnérable aux attaques de reconnaissance. configurez **no lldp run** pour désactiver LLDP globalement. Pour désactiver LLDP sur l'interface, configurez **no lldp transmit** et **no lldp receive**.


## ==Remarque==

>[!INFO]
>Ces solutions de couche 2 ne seront pas efficaces si les protocoles de gestion ne sont pas sécurisés. Par exemple, les protocoles de gestion Syslog, Protocole de gestion de réseau simple (SNMP), Protocole de transfert de fichiers trivial (TFTP), telnet, Protocole de transfer de fichier (FTP) et la plupart des autres protocoles courants ne sont pas sécurisés; par conséquent, les stratégies suivantes sont recommandées:
>- Utilisez toujours des variantes sécurisées de ces protocoles telles que SSH, Protocole de copie sécuriséel (SCP), FTP sécurisé (SFTP) et couche de socket sécurisée / Sécurité de la couche de transport (SSL / TLS).
>- Envisagez d'utiliser un réseau de gestion hors bande pour gérer les périphériques.
>- Utilisez un VLAN de gestion dédié sur lequel seul circule le trafic de gestion.
>- Utilisez des listes de contrôle d'accès pour filtrer tout accès non souhaité.



