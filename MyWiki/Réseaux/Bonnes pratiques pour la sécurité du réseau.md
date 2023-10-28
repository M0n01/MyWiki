
La sécurité du réseau consiste à protéger les informations et les systèmes d'information contre tout accès, utilisation, divulgation, interruption, modification ou destruction non autorisés.

La plupart des organisations suivent la triade de sécurité de l'information de la CIA:

- **Confidentialité**: 
>Seuls les personnes, entités ou processus autorisés peuvent accéder aux informations sensibles. Cela peut nécessiter l'utilisation d'algorithmes de cryptage cryptographiques tels que AES pour crypter et décrypter les données.

- **Intégrité** 
>Désigne la protection des données contre toute altération non autorisée. Il nécessite l'utilisation d'algorithmes de hachage cryptographiques tels que SHA.

- **Disponibilité** 
>Les utilisateurs autorisés doivent avoir un accès ininterrompu aux ressources et données importantes. Cela nécessite la mise en œuvre de services, de passerelles et de liaisons redondantes.


## La défense en profondeur

La plupart des organisations utilisent une approche de défense en profondeur de la sécurité. Ceci est également connu comme une approche en couches. Cela nécessite une combinaison de périphériques réseau et de services fonctionnant ensemble. Prenons l'exemple du réseau de la figure.

![[DefProf.png]]


Plusieurs dispositifs et services de sécurité sont mis en œuvre pour protéger les utilisateurs et les atouts d’une organisation contre les menaces TCP / IP.

- **VPN** 
>Un routeur est utilisé pour fournir des services VPN sécurisés avec des sites d'entreprise et un support d'accès à distance pour les utilisateurs distants à l'aide de tunnels cryptés sécurisés.

- **Pare-feu ASA**
>Cet appareil dédié fournit des services de pare-feu avec état. Il garantit que le trafic interne peut sortir et revenir, mais le trafic externe ne peut pas établir de connexions avec des hôtes internes.

- **IPS** 
>Un système de prévention des intrusions (IPS) surveille le trafic entrant et sortant à la recherche de logiciels malveillants, de signatures d'attaques réseau, etc. S'il détecte une menace, il peut immédiatement l'arrêter.

- **ESA/WSA** 
>L'appliance de sécurité de messagerie (ESA) filtre les spams et les e-mails suspects. L'appliance de sécurisation du web filtre les sites de malwares Internet connus et suspects.

- **Serveur AAA** 
>Ce serveur contient une base de données sécurisée de qui est autorisé à accéder et à gérer les périphériques réseau. Les périphériques réseau authentifient les utilisateurs administratifs à l'aide de cette base de données.


## Pare-feu

Un pare-feu est un système, ou un groupe de systèmes, qui impose une politique de contrôle d'accès entre des réseaux.

**Tous les pare-feu partagent certaines propriétés communes:**

- Résistent aux attaques réseau.
- Sont les seuls points de transit entre les réseaux d'entreprise internes et les réseaux externes car tout le trafic passe par le pare-feu.
- Appliquent la politique de contrôle d'accès.

**Il y a plusieurs avantages à utiliser un pare-feu dans un réseau :**

- Il empêche les utilisateurs non fiables d'accéder aux hôtes, aux ressources et aux applications sensibles.
- Il assainit le flux de protocoles pour empêcher l'exploitation des failles.
- Il bloque les données malveillantes provenant des serveurs et des clients.
- Il simplifie la gestion de la sécurité en confiant la majeure partie du contrôle d'accès réseau à quelques pare-feu sur le réseau.


## IPS

Pour vous défendre contre les attaques rapides et évolutives, vous pouvez avoir besoin de systèmes de détection et de prévention économiques, tels que les systèmes de détection des intrusions (IDS) ou les systèmes de prévention des intrusions plus évolutifs (IPS). L'architecture du réseau intègre ces solutions dans les points d'entrée et de sortie du réseau.

Les technologies IDS et IPS partagent plusieurs caractéristiques. Les technologies IDS et IPS sont toutes deux déployées comme des capteurs. Des capteurs IDS ou IPS peuvent se présenter sous la forme de différents dispositifs:

- Un routeur configuré avec le logiciel Cisco IOS IPS
- Un appareil spécialement conçu pour fournir des services IDS ou IPS dédiés
- Un module réseau installé dans un ASA (Adaptive Security Appliance), un commutateur ou un routeur

Les technologies IDS et IPS détectent les modèles de trafic réseau à l'aide de signatures. Une signature est un ensemble de règles qu'un IDS ou un IPS utilise pour détecter les activités malveillantes. Les signatures peuvent être utilisées pour détecter les violations graves de la sécurité, détecter les attaques de réseau courantes et recueillir des informations. Les technologies IDS et IPS peuvent détecter les modèles de signature atomiques (paquet unique) ou composites (multipaquets).


## Appliances de Sécurité du Contenu

Les dispositifs de sécurité du contenu incluent un contrôle précis des e-mails et de la navigation Web pour les utilisateurs d'une organisation.

#### Appliance de sécurité de messagerie Cisco (ESA)

Appliance de sécurité de messagerie Cisco ESA (Cisco Email Security Appliance) est un appareil spécial conçu pour surveiller le protocole de transfert de courrier simple SMTP (Simple Mail Transfer Protocol). Cisco ESA est constamment mis à jour par des flux en temps réel de Cisco Talos, qui détecte et corrèle les menaces et les solutions en utilisant un système de surveillance de base de données mondial. Ces données de renseignement sur les menaces sont extraites par Cisco ESA toutes les trois à cinq minutes.

![[ESA.png]]


#### Appliance de sécurité Web Cisco (WSA)

L'appliance de sécurité Web Cisco (WSA) est une technologie d'atténuation des menaces Web. Cisco WSA combine une protection avancée contre les logiciels malveillants, la visibilité et le contrôle des applications, des contrôles de politique d'utilisation acceptable et des rapports.
Certaines fonctionnalités et applications, comme le chat, la messagerie, la vidéo et l’audio, peuvent être autorisées, limitées avec le temps et de bande passante, ou bloquées, selon les besoins de l’organisation. Le WSA peut effectuer la mise sur liste noire des URL, le filtrage des URL, l'analyse des logiciels malveillants, la catégorisation des URL, le filtrage des applications Web et le chiffrement et le déchiffrement du trafic Web. Il s'emploi comme l'ESA dans la figure au-dessus


