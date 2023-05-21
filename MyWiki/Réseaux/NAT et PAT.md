
## NAT

==**NAT**== : Traductions d'adresses. Lorsque l'on a une seul adresse public pour plusieurs adresses privé. Consiste à limiter la consommation des adresses IPv4 publiques. Permet également d'ajouter un niveau de confidentialité et de sécurité à un réseau, car elle empêche les réseaux externes de voir les adresses IPv4 internes.

**==Pool NAT==** : Groupe d'adresses IPv4 publiques. Lorsqu'un périphérique interne envoie du trafic hors du réseau, le routeur configuré pour la NAT traduit l'adresse IPv4 interne du périphérique en une adresse publique du pool NAT. Pour les périphériques externes, tout le trafic entrant sur le réseau et sortant de celui-ci semble posséder une adresse IPv4 publique du pool d'adresses fourni.

![[NAT.png]]

La fonction NAT comprend quatre types d'adresses:

- **Adresse locale interne** : L'adresse de la source vue du réseau interne. Il s'agit généralement d'une adresse IPv4 privée. Sur la figure, l'adresse IPv4 192.168.10.10 est attribuée à PC1. Il s'agit de l'adresse locale interne de PC1.

- **Adresse globale interne** : L'adresse de la source vue du réseau externe. Il s'agit généralement d'une adresse IPv4 routable globalement. Sur la figure, lorsque le trafic provenant de PC1 est envoyé au serveur Web sur 209.165.201.1, R2 traduit l'adresse locale interne en adresse globale interne. Dans ce cas, R2 modifie l'adresse IPv4 source, qui passe de 192.168.10.10 à 209.165.200.226. Dans la terminologie NAT, l'adresse locale interne 192.168.10.10 est traduite en adresse globale interne 209.165.200.226.

- **Adresse locale externe** : L'adresse de destination vue du réseau externe. Dans cet exemple, PC1 envoie le trafic au serveur Web à l'adresse IPv4 209.165.201.1. Bien que cela soit rarement le cas, cette adresse peut être différente de l'adresse de destination globalement routable.

- **Adresse globale externe** : L'adresse de destination vue du réseau externe. Il s'agit d'une adresse IPv4 globalement routable attribuée à un hôte sur l'internet. Par exemple, le serveur Web est accessible à l'adresse IPv4 209.165.201.1. Le plus souvent, les adresses locale et globale externes sont identiques.

> **==Inconvénients NAT==** : 
	- **Performances du réseau**, en particulier pour les protocoles en temps réel tels que la voix sur IP. La fonction NAT augmente les délais de transfert, car la traduction de chaque adresse IPv4 des en-têtes de paquet prend du temps.
	- **Perte de l'adressage de bout en bout**. De nombreux protocoles et applications Internet dépendent de l'adressage de bout en bout de la source à la destination. Certaines applications ne sont pas compatibles avec la NAT. Par exemple, certaines applications de sécurité, telles que les signatures numériques échouent, car l'adresse IPv4 source change avant d'atteindre la destination. Les applications qui utilisent des adresses physiques au lieu d'un nom de domaine qualifié n'atteignent pas les destinations qui sont traduites sur le routeur NAT. Ce problème peut parfois être évité par l'utilisation de mappages NAT statiques.
	- **Complique l’utilisation des protocoles de tunneling, tels qu’IPsec**, car la NAT modifie les valeurs dans les en-têtes, provoquant ainsi l’échec des vérifications d’intégrité.


### NAT statique

La NAT statique utilise un mappage de type un à un des adresses locales et globales. Ces mappages sont configurés par l'administrateur réseau et restent constants.

La NAT statique est particulièrement utile pour les serveurs Web ou les périphériques qui doivent posséder une adresse permanente accessible depuis l'internet, notamment les serveurs Web d'entreprise. Elle sert également aux périphériques qui doivent être accessibles à distance par le personnel autorisé, mais pas par tous les utilisateurs d'internet.

La NAT statique nécessite qu'il existe suffisamment d'adresses publiques disponibles pour satisfaire le nombre total de sessions utilisateur simultanées.


### NAT dynamique

La NAT dynamique utilise un pool d'adresses publiques et les attribue selon la méthode du premier arrivé, premier servi. Lorsqu'un périphérique interne demande l'accès à un réseau externe, la NAT dynamique attribue une adresse IPv4 publique disponible du pool.

Comme la fonction NAT statique, la NAT dynamique nécessite qu'il existe suffisamment d'adresses publiques disponibles pour satisfaire le nombre total de sessions utilisateur simultanées.


### PAT

La traduction d'adresses de port (PAT), également appelée surcharge NAT, mappe plusieurs adresses IPv4 privées à une seule adresse IPv4 publique unique ou à quelques adresses. C’est ce que font la plupart des routeurs personnels. Le FAI attribue une adresse au routeur et pourtant, plusieurs membres de la famille peuvent accéder simultanément à l'internet. C'est la forme la plus courante de NAT pour la maison et l'entreprise.

Grâce au PAT, plusieurs adresses peuvent être mappées à une ou quelques adresses, car chaque adresse privée est également identifiée par un numéro de port. Lorsqu’un périphérique lance une session TCP/IP, il génère une valeur de port source TCP ou UDP ou un ID de requête attribué spécialement au ICMP, pour identifier uniquement la session. Lorsque le routeur NAT reçoit un paquet du client, il utilise son numéro de port source pour identifier de manière unique la traduction NAT spécifique.

La fonction PAT tente de conserver le port source d’origine. Cependant, si le port source d'origine est déjà utilisé, la PAT attribue le premier numéro de port disponible en commençant au début du groupe de ports approprié 0 à 511, 512 à 1023 ou 1024 à 65535. Lorsqu'il n'y a plus de ports disponibles et que le pool d'adresses comporte plusieurs adresses externes, la PAT passe à l'adresse suivante pour essayer d'attribuer le port source d'origine. Ce processus se poursuit jusqu’à ce qu’il n’y ait plus de ports ou d’adresses IPv4 externes disponibles.

>Qu'en est-il des paquets IPv4 transportant des données autres qu'un segment TCP ou UDP? 
>
>Ces paquets ne comportent pas de numéro de port de couche 4. La fonction PAT traduit la plupart des protocoles communs transportés par IPv4 qui n'utilisent pas le protocole TCP ou UDP comme protocole de couche transport. Le plus connu de ces derniers est ICMPv4. Chacun de ces types de protocole est pris en charge différemment par la PAT. Par exemple, les messages de requête, les demandes d'écho et les réponses d'écho ICMPv4 incluent un ID de requête. ICMPv4 utilise l'ID de requête pour identifier une demande d'écho et sa réponse d'écho correspondante. L'ID de requête est incrémenté à chaque envoi de demande d'écho. La fonction PAT utilise l'ID de requête au lieu du numéro de port de couche 4.


## Tableau Récapitulatif

<table>
	<tr>
		<th>NAT</th>
		<th>PAT</th>
	</tr>
	<tr>
		<td>Un mappage un-à-un entre une adresse locale interne et une adresse globale interne.</td>
		<td>Une adresse globale interne peut être mappée à de nombreuses adresses locale interne.</td>
	</tr>
	<tr>
		<td>Utilise uniquement les adresses IPv4 dans le processus de traduction.</td>
		<td>Utilise les adresses IPv4 et les numéros de port source TCP ou UDP dans la processus de traduction.</td>
	</tr>
	<tr>
		<td>Une adresse globale interne unique est requise pour chaque hôte interne accédant au réseau externe.</td>
		<td>Une seule adresse globale interne unique peut être partagée par plusieurs hôtes internes accédant au réseau externe.</td>
	</tr>
</table>


