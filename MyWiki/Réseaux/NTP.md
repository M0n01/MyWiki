Il est important de synchroniser l'heure sur tous les périphériques du réseau, car tous les aspects de gestion, sécurisation, dépannage et planification des réseaux nécessitent un horodatage précis. Si l'heure n'est pas synchronisée entre les différents périphériques, il vous sera impossible de déterminer l'ordre des événements et leurs causes.

>[!Remarque]
Le protocole NTP utilise le port UDP 123 et est décrit dans le document RFC 1305.

Lorsque le protocole NTP est mis en œuvre sur le réseau, il peut être configuré de sorte à se synchroniser à une horloge principale privée ou à un serveur NTP publiquement disponible sur Internet.

Les réseaux NTP utilisent un système de sources temporelles hiérarchique. Chaque niveau de ce système hiérarchique est appelé strate. Le niveau de strate correspond au nombre de sauts à partir de la source faisant autorité. L'heure synchronisée est diffusée sur le réseau à l'aide du protocole NTP.

![[NTP1.png]]

Les serveurs NTP sont disposés en trois niveaux montrant les trois strates. La strate 1 est connectée aux horloges de la strate 0.

|                           |                                                                                                                                                                                                                                                                                                                                |
| ------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| **Strate 0**              |    Un réseau NTP obtient l'heure à partir sources temporelles faisant autorité. Ces sources, également appelées périphériques de strate 0, sont des périphériques de suivi horaire haute précision censés être exacts et sans ou peu de retard. Les périphériques de strate 0 sont représentés par l'horloge sur la figure.    |
| **Strate 1**              |                                                                               Les périphériques de strate 1 sont directement connectés aux sources temporelles faisant autorité. Ils représentent la principale référence temporelle du réseau.                                                                                |
| **Strate 2 et inférieur** | Les serveurs de strate 2 sont connectés aux périphériques de strate 1 au moyen de connexions réseau. Les périphériques de strate 2, tels que les clients NTP, synchronisent leur horloge à l'aide des paquets NTP des serveurs de la strate 1. Ils peuvent également servir de serveurs pour les périphériques de la strate 3. |

Le nombre de sauts maximal est de 15. La strate 16, le niveau de strate le plus bas, indique qu'un périphérique n'est pas synchronisé.






