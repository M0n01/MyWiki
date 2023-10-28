

## Attaques de Reconnaissance

#### Effectuer une requête d'informations sur une cible

L'acteur de menace recherche les premières informations sur une cible. Divers outils peuvent être utilisés, dont la recherche Google, ==**whois**== et plus encore.

#### Lancer un balayage ping du réseau cible

La recherche d'informations révèle généralement l'adresse du réseau cible. L'acteur de menace peut désormais lancer un balayage ping pour déterminer quelles adresses IP sont actives.

#### Lancer l'analyse des ports des adresses IP actives

C'est utilisé pour déterminer quels ports ou services sont disponibles. Exemples de scanners de ports: ==**Nmap, SuperScan, Angry IP Scanner et NetScanTools**==.

#### Exécuter des scanners de vulnérabilité

Il s'agit d'interroger les ports identifiés pour déterminer le type et la version de l'application et du système d'exploitation qui s'exécutent sur l'hôte. Des exemples d'outils incluent ==**Nipper, Secuna PSI, Core Impact, Nessus v6, SAINT, et Open VAS**==.

#### Exécuter des outils d'exploitation

L'acteur de menace tente maintenant de découvrir des services vulnérables qui peuvent être exploités. Il existe une variété d'outils d'exploitation de la vulnérabilité y compris ==**Metasploit, Core Impact, Sqlmap, Social Engineer Toolkit et Netsparker**==.


## Attaques IP

#### Attaques ICMP

Les acteurs de menace utilisent des paquets d'écho (ping) ICMP (Internet Control Message Protocol) pour découvrir les sous-réseaux et les hôtes sur un réseau protégé, pour générer des attaques par inondation DoS et pour modifier les tables de routage des hôtes.

#### Attaques d'amplification et de réflexion (attaque Schtroumpf)

Les acteurs de menace tentent d'empêcher les utilisateurs légitimes d'accéder aux informations ou aux services à l'aide d'attaques DoS et DDoS.
- **Amplification** : L'auteur de la menace transfère les messages de demande d'écho ICMP à de nombreux hôtes. Ces messages contiennent l'adresse IP source de la victime.
- **Réflexion** : Ces hôtes répondent tous à l'adresse IP usurpé de la victime pour la submerger.

>[!Remarque]
De nouvelles formes d'attaques d'amplification et de réflexion telles que les attaques de réflexion et d'amplification basées sur DNS et les attaques d'amplification NTP (Network Time Protocol) sont désormais utilisées.

#### Attaques par usurpation d'adresse

Les acteurs de menace usurpent l'adresse IP source dans un paquet IP pour effectuer une usurpation aveugle ou une usurpation non aveugle.
- **Usurpation d'identité non aveugle** : L'acteur de menace peut voir le trafic qui est envoyé entre l'hôte et la cible. L'acteur de menace utilise une usurpation d'identité non aveugle pour inspecter le paquet de réponse de la victime cible. L'usurpation non aveugle détermine l'état d'un pare-feu et la prédiction du numéro de séquence. Il peut également détourner une session autorisée.
- **Usurpation aveugle** : L'acteur de menace ne peut pas voir le trafic qui est envoyé entre l'hôte et la cible. L'usurpation aveugle est utilisée pour les attaques DoS.

>[!Remarque]
>L'usurpation d'identité est généralement intégrée à une autre attaque telle qu'une attaque de Schtroumpf.

#### Attaques de l'homme-au-milieu (MITM)

Les acteurs de menace se positionnent entre une source et une destination pour surveiller, capturer et contrôler de manière transparente la communication. Ils pourraient espionner en inspectant les paquets capturés, ou modifier les paquets et les transmettre à leur destination d'origine.

#### Empoisonnement de cache ARP

L'attaque d'empoisonnement ARP peut être passive ou active. L'empoisonnement passif par ARP est l'endroit où les acteurs de menace volent des informations confidentielles. L'empoisonnement ARP actif est l'endroit où les acteurs de menace modifient les données en transit ou injectent des données malveillantes.
Il existe de nombreux outils pour créer des attaques ARP MITM, notamment **==dsniff, Cain & Abel, ettercap, Yersinia==** et autres.

#### Détournement de session

Les acteurs de menace accèdent au réseau physique, puis utilisent une attaque MITM pour détourner une session.

#### Attaques DNS

###### Attaques résolveur ouvert DNS

De nombreuses organisations utilisent les services de serveurs DNS ouverts au public tels que GoogleDNS (8.8.8.8) pour fournir des réponses aux requêtes. Ce type de serveur DNS est appelé résolveur ouvert. Un résolveur ouvert DNS répond aux requêtes des clients en dehors de son domaine administratif. Les résolveurs ouverts DNS sont vulnérables à plusieurs activités malveillantes.

- **Attaques d'empoisonnement du cache DNS**
>Les acteurs de menace envoient des informations falsifiées sur les ressources d'enregistrement (RR) vers un résolveur DNS pour rediriger les utilisateurs de sites légitimes vers des sites malveillants.

- **Attaques par amplification et réflexion du DNS**
>Les acteurs de menace utilisent des attaques DoS ou DDoS sur les résolveurs ouverts DNS pour augmenter le volume des attaques et masquer la véritable source d'un attaque.

- **Attaques sur l'utilisation des ressources du DNS**
>Une attaque DoS qui consomme les ressources des résolveurs ouverts DNS pour négativement affecter ses opérations. L'impact de cette attaque peut nécessiter le redémarrage du résolveur ouvert DNS ou des services pour être arrêté et redémarré.

###### Attaques furtive DNS

Pour cacher leur identité, les acteurs de menace utilisent également les techniques de furtivité DNS pour mener leurs attaques.

- **Flux rapide**
>Le fast fluxing DNS est une technique qui consiste à associer plusieurs adresses IP à un seul nom de domaine et à changer rapidement ces adresses IP. Parfois, des centaines, voire des milliers d'adresses IP sont utilisées. Les attaquants utilisent le fast-fluxing DNS pour maintenir leurs propriétés web en état de fonctionnement, cacher la véritable origine de leur activité malveillante et empêcher les équipes de sécurité de bloquer leur adresse IP. Cette technique est couramment utilisée par les botnets.

- **Double flux IP**
>Le double fast-flux ajoute une autre couche de flux DNS, ce qui rend encore plus difficile le blocage d'un domaine et la recherche de l'origine d'une activité malveillante. Avec le double fast fluxing, l'adresse IP du serveur de noms faisant autorité est également modifiée rapidement.

- **Algorithmes de génération de domaine**
>Les auteurs de menace utilisent cette technique dans les logiciels malveillants pour générer aléatoirement des noms de domaine qui peuvent ensuite être utilisés comme points de rendez-vous pour leur serveurs de commande et de contrôle (C&C).

###### Les attaques de shadowing de domaine DNS

L'observation de domaine implique que l'acteur de menace collecte les informations d'identification du compte de domaine afin de créer silencieusement plusieurs sous-domaines à utiliser pendant les attaques. Ces sous-domaines pointent généralement vers des serveurs malveillants sans alerter le propriétaire réel du domaine parent.

###### Attaques de Tunnellisation (tunneling) DNS

Les acteurs de menace qui utilisent la tunnellisation DNS placent le trafic non DNS dans le trafic DNS. Cette méthode contourne souvent les solutions de sécurité lorsqu'un acteur de menace souhaite communiquer avec des robots à l'intérieur d'un réseau protégé, ou exfiltrer des données de l'organisation, comme une base de données de mots de passe. Lorsque l'acteur de menace utilise la tunnellisation DNS, les différents types d'enregistrements DNS sont modifiés. Voici comment fonctionne la tunnelisation DNS pour les commandes CnC envoyées à un botnet:

1.  Les données de commande sont divisées en plusieurs blocs codés.
2.  Chaque bloc est placé dans une étiquette de nom de domaine de niveau inférieur de la requête DNS.
3.  Étant donné qu'il n'y a pas de réponse du DNS local ou en réseau pour la requête, la demande est envoyée aux serveurs DNS récursifs du FAI.
4.  Le service DNS récursif transmet la requête au serveur de noms faisant autorité de l'acteur de menace.
5.  Le processus est répété jusqu'à ce que toutes les requêtes contenant les blocs soient envoyées.
6.  Lorsque le serveur de noms faisant autorité de l'acteur de menace reçoit les requêtes DNS des appareils infectés, il envoie des réponses pour chaque requête DNS, qui contiennent les commandes CnC encapsulées et encodées.
7.  Le malware sur l'hôte compromis recombine les blocs et exécute les commandes cachées dans l'enregistrement DNS.

Pour arrêter les attaques DNS par tunnellisation, il faut utiliser un filtre inspectant le trafic DNS. Portez une attention particulière aux requêtes DNS qui sont plus longues que la moyenne ou à celles qui ont un nom de domaine suspect. Les solutions DNS, comme Cisco OpenDNS, bloquent une grande partie du trafic de DNS par tunnellisation en identifiant les domaines suspects.

#### Attaques DHCP

###### Attaques par usurpation DHCP

Une attaque d'usurpation DHCP se produit lorsqu'un serveur DHCP non autorisé est connecté au réseau et fournit de faux paramètres de configuration IP aux clients légitimes. Un serveur non autorisé peut fournir une variété d'informations trompeuses:

- **Passerelle par défaut incorrect**  
>L'acteur de menace fournit une passerelle non valide ou l'adresse IP de son hôte pour créer une attaque MITM. Cela peut ne pas être détecté car l'intrus intercepte le flux de données à travers le réseau.

- **Serveur DNS incorrect** 
>L'acteur de menace fournit une adresse de serveur DNS incorrecte pointant l'utilisateur vers un site Web malveillant.

- **Adresse IP incorrecte** 
>L'acteur de menace fournit une adresse IP non valide, une adresse IP de passerelle par défaut non valide, ou les deux. L'acteur de menace crée ensuite une attaque DoS sur le client DHCP.


## Attaques ICMP

#### Demande et réponse d'écho ICMP

Ceci est utilisé pour effectuer une vérification de l'hôte et des attaques DoS.

#### ICMP inaccessible

Il est utilisé pour effectuer des attaques de reconnaissance et de balayage de réseau.

#### Réponse de masque ICMP

Ceci est utilisé pour mapper un réseau IP interne.

#### Redirection ICMP

Ceci est utilisé pour attirer un hôte cible dans l'envoi de tout le trafic via un appareil compromis et créer une attaque MITM.

#### Découverte du routeur ICMP

Ceci est utilisé pour injecter des entrées de route fausses dans la table de routage d'un hôte cible.


## Attaques TCP

#### Attaque par inondation TCP SYN

Exploite la négociation à trois voies TCP. Un acteur de menace envoye continuellement des paquets de demande de session TCP SYN avec une adresse IP source falsifiée de manière aléatoire à une cible. Le périphérique cible répond avec un paquet TCP SYN-ACK à l'adresse IP usurpée et attend un paquet TCP ACK. Ces réponses n'arrivent jamais. Finalement, l'hôte cible est submergé de connexions TCP semi-ouvertes et les services TCP sont refusés aux utilisateurs légitimes.

#### Attaque de réinitialisation du TCP

Une attaque de réinitialisation TCP peut être utilisée pour mettre fin aux communications TCP entre deux hôtes. TCP utilise un échange à quatre voies pour fermer la connexion TCP à l'aide d'une paire de segments FIN et ACK de chaque point de terminaison TCP. Une connexion TCP se termine lorsqu'elle reçoit un bit RST. Il s'agit d'une manière abrupte de rompre la connexion TCP et d'informer l'hôte destinataire de cesser immédiatement d'utiliser la connexion TCP. Un acteur de menace pourrait effectuer une attaque de réinitialisation TCP et envoyer un paquet usurpé contenant un TCP RST à un ou aux deux points de terminaison.

#### Détournement de session TCP

Le détournement de session TCP est une autre vulnérabilité TCP. Bien que difficile à mener, un acteur de menace prend le contrôle d'un hôte déjà authentifié lors de sa communication avec la cible. L'acteur de menace doit usurper l'adresse IP d'un hôte, prédire le numéro de séquence suivant et envoyer un ACK à l'autre hôte. En cas de succès, l'acteur de menace pourrait envoyer, mais pas recevoir, des données de l'appareil cible.


## Attaques UDP

#### Attaque par inondation UDP

Dans une attaque par inondation UDP, toutes les ressources d'un réseau sont consommées. L'acteur de menace doit utiliser un outil comme ==**UDP Unicorn**== ou ==**Low Orbit Ion Cannon**==. Ces outils envoient un flot de paquets UDP, souvent à partir d'un hôte usurpé, vers un serveur du sous-réseau. Le programme balaiera tous les ports connus en essayant de trouver des ports fermés. Cela fera répondre le serveur avec un message inaccessible du port ICMP. Étant donné qu'il existe de nombreux ports fermés sur le serveur, cela crée beaucoup de trafic sur le segment, qui utilise la majeure partie de la bande passante. Le résultat est très similaire à une attaque DoS.