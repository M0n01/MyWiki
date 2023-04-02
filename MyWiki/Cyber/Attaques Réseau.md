

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

#### Détournement de session

Les acteurs de menace accèdent au réseau physique, puis utilisent une attaque MITM pour détourner une session.


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