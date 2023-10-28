Les composants de votre réseau peuvent vous dire si quelque chose ne va pas. Le protocole syslog a été conçu pour vous assurer que vous pouvez recevoir et comprendre ces messages.

La méthode d'accès aux messages système la plus couramment utilisée est un protocole appelé Syslog.

Syslog est un terme utilisé pour décrire une norme. Il sert également à décrire le protocole développé pour cette norme. Le protocole Syslog a été développé pour les systèmes UNIX dans les années 1980, mais a été documenté pour la première fois dans la RFC 3164 par l'IETF en 2001. Le protocole Syslog utilise le port UDP 514 pour envoyer des messages de notification d'événement sur des réseaux IP à des collecteurs de messages d'événement, comme le montre la figure.

![[Syslog1.png]]

Le service de journalisation du protocole Syslog assume trois fonctions principales,comme suivant:

- **La capacité à collecter les informations de journalisation pour la surveillance et le dépannage**
- **La capacité de sélectionner le type d'information de journalisation capturée**
- **La capacité à spécifier les destinations des messages Syslog capturés**


## Fonctionnement

Des messages Syslog soient envoyés sur le réseau vers un serveur Syslog externe. Ces messages peuvent être récupérés sans qu'il soit nécessaire d'accéder au périphérique réel.

De même, des messages Syslog peuvent également être envoyés vers un tampon interne. Les messages envoyés vers le tampon interne ne peuvent être affichés que par l'intermédiaire de l'interface en ligne de commande du périphérique.

Enfin, l'administrateur réseau peut spécifier que seuls certains types de messages sont envoyés à différentes destinations. Il est par exemple possible de configurer le périphérique de telle sorte qu'il transfère tous les messages système vers un serveur Syslog externe. Toutefois, les messages de niveau de débogage sont transférés vers le tampon interne et ne sont accessibles que par l'intermédiaire de l'administrateur à partir de l'interface en ligne de commande.

Les destinations populaires des messages Syslog sont les suivantes:

- **Tampon de journalisation (mémoire vive à l'intérieur d'un routeur ou d'un commutateur)**
- **Ligne de console**
- **Ligne de terminal**
- **Serveur Syslog**


## Format de message

Les périphériques Cisco génèrent des messages Syslog à la suite des événements réseau. Chaque message Syslog contient un niveau de gravité et une capacité.

| **Nom de gravité** | **Niveau de gravité** | **Explication**                 |
| ------------------ | --------------------- | ------------------------------- |
| Urgence            | Niveau 0              | Système inutilisable            |
| Alerte             | Niveau 1              | Action immédiate requise        |
| Essentiel          | Niveau 2              | Condition critique              |
| Erreur             | Niveau 3              | Condition d'erreur              |
| Avertissement      | Niveau 4              | Condition d'avertissement       |
| Notification       | Niveau 5              | Événement normal mais important |
| Informatif         | Niveau 6              | Message informatif              |
| Débogage           | Niveau 7              | Message de débogage             |

Chaque niveau Syslog a sa propre signification:

- **Avertissement Niveau 4 - Urgence Niveau 0**
>Ces messages sont des messages d'erreur relatifs aux dysfonctionnements logiciels ou matériels; ces types de messages signifient que la fonctionnalité du périphérique est affectée. La gravité du problème détermine le niveau Syslog réel appliqué.

- **Notification Niveau 5**
>Ce niveau de notification concerne les événements normaux mais importants. Par exemple, les transitions d'interface à l'état «up» ou «down» ainsi que les messages de redémarrage du système s'affichent au niveau de notification.

- **Message informatif Niveau 6** 
>Cela est un message d'informations normal qui n'affecte pas le fonctionnement de l'appareil. Par exemple, lors du démarrage d'un appareil Cisco, vous pouvez afficher le message informationnel suivant: %LICENSE-6-EULA_ACCEPT_ALL: The Right to Use End User License Agreement is accepted.

- **Message de débogage Niveau 7** 
>Ce niveau indique que les messages sont des résultats générés suite à l'exécution de diverses commandes **debug** .









