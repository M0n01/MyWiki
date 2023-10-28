Les API se trouvent presque partout. Amazon Web Services, Facebook et les appareils domotiques tels que les thermostats, les réfrigérateurs et les systèmes d'éclairage sans fil utilisent tous des API. Ils sont également utilisés pour construire une automatisation de réseau programmable.

Une API est un logiciel qui permet à d'autres applications d'accéder à ses données ou services. Il s'agit d'un ensemble de règles décrivant comment une application peut interagir avec une autre et les instructions permettant à l'interaction de se produire. L'utilisateur envoie une demande d'API à un serveur demandant des informations spécifiques et reçoit une réponse d'API en retour du serveur avec les informations demandées.

## Exemple d'API

- Réservations de compagnies aériennes :

1) La première option utilise le site Web d'une compagnie aérienne spécifique. En utilisant le site Web de la compagnie aérienne, l’utilisateur entre les informations pour effectuer une demande de réservation. Le site Web interagit directement avec la propre base de données de la compagnie aérienne et fournit à l’utilisateur des informations correspondant à sa demande.

![[API1.png]]

2) Au lieu d'utiliser un site Web individuel de compagnie aérienne qui a un accès direct à ses propres informations, il existe une deuxième option. Les utilisateurs peuvent utiliser un site de voyage pour accéder à ces mêmes informations, non seulement d'une compagnie aérienne spécifique mais d'une variété de compagnies aériennes. Dans ce cas, l'utilisateur entre dans des informations de réservation similaires. Le site Web du service de voyage interagit avec les différentes bases de données des compagnies aériennes à l'aide des API fournies par chaque compagnie aérienne. Le service de voyage utilise chaque API de compagnie aérienne pour demander des informations à cette compagnie aérienne spécifique, puis il affiche les informations de toutes les compagnies aériennes sur sa page Web.

![[API2.png]]

L'API agit comme une sorte de messager entre l'application de requête et l'application sur le serveur qui fournit les données ou le service. Le message de l'application de requête au serveur sur lequel résident les données est appelé appel API.

## API Ouvertes, Internes et Partenaires

Une considération importante lors du développement d'une API est la distinction entre les API ouvertes, internes et partenaires:

- **API ouvertes ou API publiques -** Accessibles au public et peuvent être utilisées sans aucune restriction. L'API de la Station spatiale internationale est un exemple d'API publique. Car ces API sont publiques, de nombreux fournisseurs d'API, tels que Google Maps, exigent que l'utilisateur obtienne une clé ou un jeton gratuit avant d'utiliser l'API. Cela permet de contrôler le nombre de demandes d'API qu'ils reçoivent et traitent. 
- **API internes ou privées -** Utilisées par une organisation ou une entreprise pour accéder aux données et services à usage interne uniquement. Un exemple d'API interne permet aux vendeurs autorisés d'accéder aux données de vente internes sur leurs périphériques mobiles.
- **API partenaires -** Utilisées entre une entreprise et ses partenaires commerciaux ou contracteurs pour faciliter les échanges entre eux. Le partenaire commercial doit disposer d'une licence ou d'une autre forme d'autorisation pour utiliser l'API. Un service de voyage utilisant l'API d'une compagnie aérienne est un exemple d'API partenaire.

## Types d'API de Service Web

Un service Web est un service disponible sur internet via le World Wide Web. Il existe quatre types d'API de service Web:

- **SOAP** : Protocole d'accès aux objets simples
	- Protocole de messagerie pour l'échange d'informations structurées XML, le plus souvent via HTTP ou SMTP (Simple Mail Transfer Protocol)
	- Lentes à analyser, complexes et rigides
	
- **REST** : Transfert d'état représentatif
	- Utilise HTTP
	- API de service Web la plus utilisée, représentant plus de 80% de tous les types d'API utilisés
	
- **XML-RPC** : Langage de balisage extensible-Appel de procédure à distance 
	- Protocole développé avant SOAP, et évolué plus tard vers ce qui est devenu SOAP
	
- **JSON-RPC** : JavaScript notation d'objet-Appel de procédure à distance
	- protocole très simple et similaire à XML-RPC

| Caractéristique      | SOAP        | REST                                            | XML-RPC                 | JSON-RPC   |
| -------------------- | ----------- | ----------------------------------------------- | ----------------------- | ---------- |
| Format de données    | XML         | JSON, XML, YAML et autres                       | XML                     | JSON       |
| Première publication | 1998        | 2000                                            | 1998                    | 2005       |
| Forces               | Bien établi | Formatage flexible et le plus largement utilisé | Bien établi, simplicité | Simplicité |

RPC est lorsqu'un système demande qu'un autre système exécute des codes et renvoie les informations. Cela se fait sans avoir à comprendre les détails du réseau. Cela fonctionne un peu comme une API REST mais il existe des différences concernant le formatage et la flexibilité.

## REST

Les navigateurs Web utilisent HTTP ou HTTPS pour demander (GET) une page Web. S'ils sont correctement demandés (code d'état HTTP 200), les serveurs Web répondent aux demandes GET avec une page Web codée HTML.

![[HTTP1.png]]

REST est un style architectural pour la conception d'applications de service Web. Il fait référence à un style d'architecture Web qui possède de nombreuses caractéristiques sous-jacentes et régit le comportement des clients et des serveurs. En termes simples, une API REST est une API qui fonctionne au-dessus du protocole HTTP. Il définit un ensemble de fonctions que les développeurs peuvent utiliser pour effectuer des requêtes et recevoir des réponses via le protocole HTTP tel que GET et POST.

La conformité aux contraintes de l'architecture REST est généralement appelée «RESTful». Une API peut être considérée comme «RESTful» si elle possède les fonctionnalités suivantes:

- **Client / serveur** - Le client gère l'extrémité avant et le serveur gère l'extrémité arrière. L'un ou l'autre peut être remplacé indépendamment de l'autre.
- **Apatride** - Aucune donnée client n'est stockée sur le serveur entre les requêtes. L'état de session est stocké sur le client.
- **Mémorisable** - Les clients peuvent mettre en cache les réponses pour améliorer les performances.

## RESTful

Un service Web RESTful est implémenté à l'aide de HTTP. Il s'agit d'une collection de ressources avec quatre aspects définis:

- L'identificateur de ressource uniforme de base (URI) pour le service Web, tel que http://example.com/resources.
- Format de données pris en charge par le service Web. Il s'agit souvent de JSON, YAML ou XML, mais il peut s'agir de tout autre format de données qui constitue une norme hypertexte valide.
- Ensemble d'opérations prises en charge par le service Web à l'aide de méthodes HTTP.
- L'API doit être basée sur l'hypertexte.

Les API RESTful utilisent des méthodes HTTP courantes, notamment POST, GET, PUT, PATCH et DELETE. Comme indiqué dans le tableau suivant, ceux-ci correspondent aux opérations RESTful: créer, lire, mettre à jour et supprimer (ou CRUD).

| Méthode HTTP | Opération RESTful |
| ------------ | ----------------- |
| POST         | Créer             |
| GET          | Lire              |
| PUT/PATCH    | Mettre à jour     |
| DELETE       | Supprimer         |

## URI, URN, et URL

Les ressources Web et les services Web tels que les API RESTful sont identifiés à l'aide d'un URI. Un URI est une chaîne de caractères qui identifie une ressource de réseau spécifique. Comme le montre la figure, un URI a deux spécialisations:

- **Nom de ressource uniforme (URN)** - identifie uniquement l'espace de noms de la ressource (page Web, document, image, etc.) sans référence au protocole.
- **Localisateur de ressources uniforme (URL)** - définit l'emplacement réseau d'une ressource spécifique sur le réseau. Les URL HTTP ou HTTPS sont généralement utilisées avec les navigateurs Web. D'autres protocoles tels que FTP, SFTP, SSH et autres peuvent utiliser une URL. Une URL utilisant SFTP peut ressembler à: sftp: //sftp.example.com.

![[URI.png]]

## Anatomie d'une Demande RESTful

Dans un service Web RESTful, une demande adressée à l'URI d'une ressource provoquera une réponse. La réponse sera une charge utile généralement formatée en JSON, mais pourrait être HTML, XML ou un autre format. La figure montre l'URI de l'API directions MapQuest. La demande d'API est pour les directions de San Jose, Californie, à Monterey, Californie.

![[RESTful.png]]

Ce sont les différentes parties de la demande d'API:

- **Serveur API** - Il s'agit de l'URL du serveur qui répond aux demandes REST. Dans cet exemple, il s'agit du serveur API MapQuest.
- **Ressources** - Spécifie l'API qui est demandée. Dans cet exemple, il s'agit de l'API directions MapQuest.
- **Requête** - Spécifie le format de données et les informations que le client demande au service API. Les requêtes peuvent inclure:
    - **Format** – Il s'agit généralement de JSON mais peut être YAML ou XML. Dans cet exemple, JSON est demandé.
    - **Clé** - La clé est pour l'autorisation, si nécessaire. MapQuest nécessite une clé pour son API de directions. Dans l'URI ci-dessus, vous devez remplacer «KEY» par une clé valide pour soumettre une demande valide.
    - **Paramètres** - Les paramètres sont utilisés pour envoyer des informations relatives à la demande. Dans cet exemple, les paramètres de requête incluent des informations sur les directions dont l'API a besoin pour qu'elle sache les directions pour retourner: "from = San + Jose, Ca" et "to = Monterey, Ca".

De nombreuses API RESTful, y compris les API publiques, nécessitent une clé. La clé est utilisée pour identifier la source de la demande. Voici quelques raisons pour lesquelles un fournisseur d'API peut nécessiter une clé:

- Pour authentifier la source pour vous assurer qu'elle est autorisée à utiliser l'API.
- Pour limiter le nombre de personnes utilisant l'API.
- Pour limiter le nombre de demandes par utilisateur.
- Pour mieux capturer et suivre les données demandées par les utilisateurs.
- Pour recueillir des informations sur les personnes utilisant l'API.

