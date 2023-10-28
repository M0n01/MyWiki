
Algorithmes de mise en file d'attente :

-   Premier entrant premier sorti (FIFO)
-   File d'attente équitable pondérée (WFQ)
-   File d'attente équitable pondérée basée sur la classe (CBWFQ)
-   File d'attente à faible latence (LLQ)

## Premier entrant premier sorti (FIFO)

Repose sur le principe du premier arrivé, premier servi, consiste à mettre le trafic en mémoire tampon et à transférer les paquets dans leur ordre d'arrivée.

![[FIFO.png]]

## File d'attente équitable pondérée (WFQ)

WFQ (Weighted Fair Queuing) est une méthode de programmation automatisée grâce à laquelle la bande passante est allouée au trafic réseau de façon équitable. WFQ n'autorise pas la configuration des options de classification. WFQ applique le principe de priorité, ou de pondération, au trafic identifié, puis le répartit en conversations ou en flux, comme illustré sur la figure.

![[WFQ.png]]

La méthode WFQ n'est ==pas prise en charge en cas de tunnélisation et de chiffrement==, car ces fonctionnalités entraînent la modification des informations de contenu des paquets dont cette méthode a besoin à des fins de classification.

Bien que la stratégie WFQ s'adapte automatiquement aux changements du trafic réseau, elle n'offre pas le même degré de précision dans le contrôle de l'allocation de la bande passante que la méthode CBWFQ.

## File d'attente équitable pondérée basée sur la classe (CBWFQ)

CBWFQ étend la fonctionnalité de mise en file d'attente pondérée (WFQ) standard afin de fournir la prise en charge des classes de trafic définies par l'utilisateur.

Avec le CBWFQ, vous définissiez des classes de trafic en fonction de critères de correspondance incluant les protocoles, les listes de contrôle d'accès (ACL) et les interfaces d'entrée. Les paquets qui répondent à ces critères pour une classe constituent le trafic pour cette classe. Chaque classe dispose de sa propre file d'attente de type FIFO. Le trafic est dirigé vers la file d'attente de sa classe, comme le montre la figure.

![[CBWFQ.png]]

Pour caractériser une classe, vous devez également spécifier sa limite de file d'attente, c'est-à-dire le nombre maximal de paquets autorisés à s'accumuler dans la file d'attente correspondant à cette classe. Les paquets faisant partie d'une classe sont soumis à la bande passante et aux limites de file d'attente qui la caractérisent.

## File d'attente à faible latence (LLQ)

Avec la fonctionnalité LLQ, la stratégie CBWFQ bénéficie d'une capacité de mise en file d'attente à priorité stricte. La priorité stricte permet aux paquets soumises à des contraintes temporelles, par exemple la voix, d'être envoyées avant les paquets présents dans d'autres files d'attente. LLQ implique une mise en file d'attente à priorité stricte, dans le cadre de CBWFQ, ce qui permet de réduire la gigue dans les conversations vocales, comme indiqué sur la figure.

![[LLQ.png]]


## Sélection d'un modèle de politique QoS approprié

### Remise au mieux

Il ne s'agit pas vraiment d'une implémentation dans la mesure où la stratégie QoS n'est pas explicitement configurée. Ce modèle est utilisé lorsque la qualité de service n'est pas nécessaire.

La conception de base d'Internet prévoit la remise des paquets «au mieux» et n'offre aucune garantie. Toujours prédominante sur Internet aujourd'hui, cette approche reste adaptée dans la plupart des cas. Comme ce modèle applique un traitement identique à tous les paquets réseau, le message vocal d'urgence est traité de la même manière qu'une photo numérique jointe à un e-mail. Sans la stratégie QoS, le réseau ne peut pas faire la différence entre les paquets et, par conséquent, ne peut pas leur appliquer un traitement préférentiel.

| Bénéfices | Inconvénients |  
| -------- | -------- |
| Il s'agit du modèle le plus évolutif.| Il n'offre aucune garantie de remise. | 
| Son évolutivité est uniquement limitée par la bande passante disponible, laquelle affecte alors l'ensemble du trafic. | Le délai et l'ordre de remise des paquets sont aléatoires et rien ne garantit leur arrivée. |
| Aucun mécanisme QoS spécial ne doit être implémenté.| Aucun paquet ne bénéficie d'un traitement préférentiel. |
| C'est le modèle le plus simple et rapide à déployer.| Les données essentielles sont traitées de la même façon que les e-mails normaux. |



### Services intégrés (IntServ)

IntServ propose une qualité de service très élevée aux paquets IP, avec remise garantie. Il définit un processus de signalisation pour que les applications puissent indiquer au réseau qu'elles nécessitent une QoS spéciale pendant une certaine période et qu'il faut réserver de la bande passante. En revanche, le modèle IntServ peut considérablement limiter l'évolutivité d'un réseau.

![[IntServ.png]]

| Bénéfices | Inconvénients |  
| -------- | -------- |
| Contrôle d'admission des ressources explicite, de bout en bout| Consommation importante de ressources due aux exigences de signalisation continue de l'architecture dynamique. | 
| Contrôle d'admission de la stratégie par demande. | Approche basée sur les flux, inadaptée aux implémentations de grande taille, par exemple Internet.|
| Signalisation des numéros de port dynamiques.|  |


### Services différenciés (DiffServ)

Ce modèle offre une implémentation QoS très flexible et évolutive. Les périphériques réseau détectent des classes de trafic et appliquent des niveaux de qualité de service spécifiques aux différentes classes de trafic.

Mécanisme simple et évolutif pour classer et gérer le trafic réseau. Peut offrir un service garanti, avec une latence faible, pour les trafics réseaux critiques tels que la voix ou la vidéo ainsi qu'une garantie de remise au mieux pour le trafic non critique, comme le trafic web ou les transferts de fichiers.
*Le modèle DiffServ peut fournir une qualité de service «quasiment garantie» tout en restant rentable et évolutif.*

Le modèle DiffServ n'est pas une stratégie QoS de bout en bout car il ne peut pas appliquer les garanties de bout en bout.

![[DiffServ.png]]

| Bénéfices | Inconvénients |  
| -------- | -------- |
| Haute évolutivité.| Aucune garantie stricte de la qualité de service. | 
| Large choix de niveaux de qualité | Nécessite le fonctionnement conjoint de mécanismes complexes sur l'ensemble du réseau|

>[!Remarque]
>Les réseaux modernes utilisent principalement le modèle DiffServ. Cependant, en raison des volumes croissants de trafic sensible à la latence et à la gigue, les modèles InteServ et RSVP sont parfois également déployés.


## Outils QoS

On distingue trois catégories d'outils QoS.

- **Outils de classification et de marquage**
>Les sessions, ou les flux, sont analysées afin de déterminer la classe de trafic à laquelle elles appartiennent.
>Une fois la classe identifiée, les paquets sont marqués.

- **Outils de prévention de l'encombrement**
>Les classes de trafic représentent des ressources réseau allouées, l'allocation étant définie dans la stratégie QoS.
>La politique de qualité de service identifie également comment certains trafics peuvent être sélectivement abandonnés, retardés ou re-marqués pour éviter les encombrements.
>Le principal outil de prévention d'encombrements est WRED et est utilisé pour réguler le trafic de données TCP en utilisant efficacement la bande passante avant que des dépassements de file d'attente

- **Outils de gestion de l'encombrement**
>Lorsque le trafic dépasse les ressources réseau disponibles, il est placé en file d'attente en attendant que des ressources se libèrent.
>Le système Cisco IOS propose plusieurs outils de gestion de l'encombrement, dont les algorithmes CBWFQ et LLQ.