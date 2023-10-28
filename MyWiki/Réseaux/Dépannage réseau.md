
## Base de référence

Une base de référence de réseau devrait répondre aux questions suivantes :

- Quelles sont les performances du réseau pendant une journée normale ou moyenne ?
- Où survient le plus grand nombre d'erreurs ?
- Quelle partie du réseau est la plus utilisée ?
- Quelle partie du réseau est la moins utilisée ?
- Quels appareils doivent être surveillés et quels seuils d'alerte doivent être définis ?
- Le réseau peut-il satisfaire les politiques identifiées ?

Permet à un administrateur de réseau de déterminer la différence entre un comportement anormal et les performances correctes du réseau, à mesure que le réseau se développe ou que les schémas de trafic changent.

#### Étape 1 - Déterminer les types de données à collecter

Sélectionner quelques variables représentant les stratégies définies. 
Sélectionnez trop de points de données = analyse difficile des données recueillies.
Quelques bonnes variables de départ sont l'utilisation de l'interface et l'utilisation du CPU.

#### Étape 2 - Identifier les dispositifs et les ports d'intérêt

Utilisez la topologie du réseau pour identifier les périphériques et les ports pour lesquels il est nécessaire de mesurer des données de performances. Les dispositifs et les ports d'intérêt sont notamment les suivants :

- Ports des périphériques réseau qui se connectent à d’autres périphériques réseau
- Serveurs
- les utilisateurs principaux
- Tout autre élément considéré comme étant critique pour le fonctionnement du système

L'interface d'un routeur ou d'un commutateur peut être une interface virtuelle, comme un périphérique SVI (interface virtuelle de commutateur).

#### Étape 3 - Déterminer la durée de base

La durée et les informations de base recueillies doivent être suffisamment longues pour déterminer une image "normale" du réseau. Il est important de surveiller les tendances quotidiennes du trafic réseau. Il est également important de surveiller les tendances qui s'établissent sur une plus longue période, par exemple à l'échelle de la semaine ou du mois. Pour cette raison, lors de la capture de données à des fins d'analyse, il faut que la période spécifiée soit au minimum de **sept jours**.

![[MesureChargeCPU.png]]

D'une manière générale, une planification initiale ne doit pas s'étendre sur une période supérieure à six semaines, sauf si des tendances spécifiques à long terme doivent être mesurées. Une planification initiale de deux à quatre semaines est généralement tout à fait adéquate.

#### Mesure des données

Lors de la documentation du réseau, il est souvent nécessaire de collecter des informations directement à partir des routeurs et des commutateurs.
Les commandes utiles évidentes de la documentation du réseau comprennent ,**ping**, **traceroute**, et **telnet,** et ainsi que les commandes **show**.
La collecte manuelle de données à l'aide de commandes **show** sur des dispositifs de réseau individuels est extrêmement longue et n'est pas une solution évolutive. La collecte manuelle de données doit être réservée aux petits réseaux ou limitée aux appareils réseau stratégiques.

## Dépannage avec les modèles en couches

![[DepannModeleCouche.png]]

## Outils logiciels de dépannage

- **Outils de système d’administration de réseaux (NMS)** : Les outils NMS (Network Management System) comprennent les outils de surveillance au niveau des appareils, de configuration et de gestion des pannes. Un logiciel de surveillance du réseau affiche de manière graphique une vue physique des périphériques réseaux qui permet aux gestionnaires du réseau de surveiller automatiquement et en continu les périphériques distants.
- **Bases de connaissances** : Doc + internet.
- **Outils de surveillance des performances réseau** : Ils permettent par exemple de dessiner des schémas de réseaux, de mettre à jour la documentation réseau matérielle et logicielle, et de mesurer de manière économique l'utilisation de la bande passante réseau de référence. **Nagios**, **Zabbix** etc.

## Analyseurs de protocoles

Les analyseurs de protocole sont utiles pour étudier le contenu des paquets pendant qu'ils circulent sur le réseau. Un analyseur de protocole décode les différentes couches de protocole dans une trame enregistrée et présente ces informations dans un format relativement facile à utiliser.
Exemple : **Wireshark**.

## Outils matériels de dépannage

- Multimètres numériques (Fluke 179)
- Testeurs de câble (Fluke LinkRunner AT)
- Analyseurs de câble (analyseur de câbles DTX de Fluke)
- Analyseurs de réseau portables (Fluke OptiView)
- Module d'analyse du réseau principal de Cisco : Le portefeuille de modules NAM (Cisco Prime Network Analysis Module), comprend du matériel et des logiciels pour l'analyse des performances dans les environnements de commutation et de routage. Il offre une interface web intégrée qui génère des rapports sur le trafic qui consomme les ressources critiques du réseau. De plus, le NAM peut capturer, décoder des paquets et suivre les délais de réponse pour identifier un problème applicatif sur un réseau ou un serveur particulier.

## Dépannage de la couche physique

![[DepannPhysique.png]]

## Dépannage de la couche liaison de données

![[DepannLDD.png]]

## Dépannage de la couche réseau

Les problèmes de la couche réseau comprennent tout problème impliquant un protocole de couche 3, tel que IPv4, IPv6, EIGRP, OSPF, etc.

![[DepannReseau.png]]

## Dépannage de la couche transport - listes de contrôle d'accès (LCA)

![[DepannTransp1.png]]

Les problèmes les plus courants avec les LCA sont dus à une mauvaise configuration, comme le montre la figure.

![[DepannTransp2.png]]

## Dépannage de la couche transport - NAT pour IPv4

Il y a plusieurs problèmes avec la NAT, comme le fait de ne pas interagir avec des services comme le DHCP et le tunneling. Il peut s'agir de NAT internes ou externes ou d'ACL mal configurés. Les autres problèmes concernent l'interopérabilité avec d'autres technologies réseau, en particulier celles qui contiennent ou qui créent des informations à partir de l'adressage du réseau hôte dans le paquet.

![[DepannTransp3.png]]

## Dépannage de la couche application

Les types de symptômes et de causes dépendent de l'application réelle elle-même.

![[DepannApp.png]]


