Le protocole SNMP (Simple Network Management Protocol) a été développé pour permettre aux administrateurs de gérer les nœuds, tels que les serveurs, les postes de travail, les routeurs, les commutateurs ainsi que les appliances de sécurité, sur un réseau IP. Ces derniers peuvent ainsi contrôler et gérer les performances du réseau, identifier et résoudre les problèmes et anticiper la croissance du réseau.

SNMP est un protocole de couche Application qui procure un format pour les messages de communication entre les gestionnaires et les agents. Le système SNMP se compose de trois éléments:

- **Gestionnaire SNMP**
- **Agents SNMP (nœud géré)**
- **Base d’informations de gestion (MIB)**

Pour configurer le protocole SNMP sur un périphérique réseau, il faut tout d'abord définir la relation entre le gestionnaire et l'agent.

Le gestionnaire SNMP fait partie d'un système de gestion du réseau (NMS). Le gestionnaire SNMP exécute le logiciel de gestion SNMP. Il peut collecter des informations à partir d'un agent SNMP à l'aide de l'action «get» et modifier des configurations sur un agent à l'aide de l'action «set». De plus, les agents SNMP peuvent transmettre des informations directement au gestionnaire de réseau en utilisant des déroutements.

![[SNMP1.png]]

L'agent SNMP et la base de données MIB sont présents sur tous les périphériques client SNMP. Les MIB contiennent les données relatives aux périphériques et à leur fonctionnement. Elles peuvent être consultées par tout utilisateur distant authentifié. C'est l'agent SNMP qui est chargé de fournir l'accès à la MIB locale.

Le protocole SNMP définit la manière selon laquelle les informations de gestion sont échangées entre les applications de gestion du réseau et les agents de gestion. Le gestionnaire SNMP interroge les agents et envoie des requêtes à la base de données MIB des agents sur le port UDP 161. Les agents SNMP envoient les déroutements SNMP au gestionnaire SNMP sur le port UDP 162.

Il existe deux types principaux de requêtes de gestionnaire SNMP, à savoir get et set.
- **get** est utilisée par le système de gestion de réseau (NMS) afin d'obtenir des données de la part d'un périphérique. 
- **set** est utilisée par le système de gestion de réseau (NMS) afin de modifier les variables de configuration du périphérique de l'agent. Une requête set peut également initier des actions au niveau d'un périphérique. Par exemple, une requête set peut provoquer le redémarrage d'un routeur ainsi que l'envoi ou la réception d'un fichier de configuration. 

Le gestionnaire SNMP utilise les actions get et set pour réaliser différentes opérations.

![[SNMP4.png]]

- Il existe également **get-bulk-request** 
>Récupère des blocs importants de données, comme plusieurs lignes dans une table, qui autrement nécessiteraient la transmission de nombreux petits blocs de données. (Fonctionne uniquement avec SNMPv2 ou version ultérieure.)

L'agent SNMP répond comme suit aux requêtes du gestionnaire SNMP:

- **Obtenir une variable MIB** 
>L'agent SNMP transmet l'information en réponse à une requête de type GetRequest-PDU envoyée par le gestionnaire du réseau. L'agent récupère la valeur de la variable MIB demandée et la transmet au gestionnaire du réseau.

- **Définir une variable MIB** 
>L'agent SNMP procède à cette opération en réponse à une requête SetRequest-PDU envoyée par le gestionnaire du réseau. L'agent SNMP remplace la valeur de la variable MIB par la valeur spécifiée par le gestionnaire du réseau. Une réponse d'agent SNMP à une requête set inclut les nouveaux paramètres définis dans le périphérique.

![[SNMP2.png]]

Pour diminuer le besoin en ressource réseau, les agents SNMP peuvent générer et envoyer des déroutements visant à informer immédiatement le système de gestion de réseau (NMS) de l'occurrence de certains événements.

![[SNMP3.png]]

Il existe plusieurs versions du protocole SNMP:

- **SNMPv1**
- **SNMPv2c**
- **SNMPv3**

Un administrateur réseau doit configurer l'agent SNMP de manière à utiliser la version SNMP prise en charge par la station de gestion. Étant donné qu'un agent peut communiquer avec plusieurs gestionnaires SNMP, il est possible de configurer le logiciel de manière à prendre en charge les communications à l'aide du protocole SNMPv1, SNMPv2c ou SNMPv3.

## Identifiants de communauté

Pour que le protocole SNMP fonctionne, il faut que le système de gestion de réseau (NMS) ait accès à la base de données MIB. Afin de s'assurer que les demandes d'accès sont valides, une forme quelconque d'authentification doit être implémentée.

Les protocoles SNMPv1 et SNMPv2c utilisent des identifiants de communauté qui contrôlent l'accès à la base de données MIB. Les identifiants de communauté sont des mots de passe qui circulent en clair (plain text). Les identifiants de communauté SNMP authentifient l'accès aux objets MIB.

Il existe deux types d'identifiants de communauté:

- **Lecture seule (ro)** 
>Ce type fournit un accès aux variables MIB, mais ne permet pas de les modifier, uniquement de les lire. La sécurité étant minimale dans la version 2c, de nombreuses entreprises utilisent le protocole SNMPv2c en mode lecture seule.

- **Lecture/écriture (rw)** 
>Ce type fournit un accès en lecture et en écriture à l'ensemble des objets de la base de données MIB.

Pour afficher ou définir des variables MIB, l'utilisateur doit spécifier l'identifiant de communauté approprié pour l'accès en lecture ou en écriture.

## ID d'objet MIB

La base de données MIB définit chaque variable comme étant un objet avec un identifiant (OID).

La figure montre des parties de la structure MIB définie par Cisco. Notez comment l'ID d'objet (OID) peut être décrit en termes de mots ou de nombres afin de pouvoir localiser facilement une variable spécifique dans l'arborescence. Les OID Cisco sont numérotés comme suit: .iso (1).org (3).dod (6).internet (1).private (4).enterprises (1).cisco (9). L'OID d'objet est donc 1.3.6.1.4.1.9.

![[MIB.png]]

À l'aide de l'utilitaire snmpget, vous pouvez récupérer manuellement des données en temps réel ou demander au NMS d'exécuter un rapport.
La figure illustre l'utilisation d'un outil snmpget gratuit, qui permet de récupérer rapidement des informations à partir de la base de données MIB.

![[MIB2.png]]



