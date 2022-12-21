## Connexion à distance

#### SSH
Protocole permettant d'établir une communication chiffrée, donc sécurisée (on parle parfois de _tunnel_), sur un réseau informatique (intranet ou Internet) entre une machine locale (le _client_) et une machine distante (le _serveur_). La sécurité du chiffrement peut être assurée par différentes méthodes, entre autre par mot de passe ou par un système de clés publique/privée (mieux sécurisé, on parle alors de [cryptographie asymétrique](https://fr.wikipedia.org/wiki/cryptographie%20asym%C3%A9trique "https://fr.wikipedia.org/wiki/cryptographie asymétrique")).

Lors d'une connexion ssh à un serveur, l'hôte sauvegarde la clé publique du serveur. Si elle vient à changer (dans le cas d'un Man In The Middle ou d'un changement sur le serveur) alors on sera prévenu de ce changement avant de se connecter.

- La clé privé doit être bien garder, elle est secrète. Si quelqu'un obtient ma clé privé, il peut se faire passer pour moi.
- Tout le monde peut avoir la clé publique, on s'en fiche. Elle ne sert qu'à chiffré des messages.

La clé publique peut être calculé avec la clé privé mais pas l'inverse.

**Déroulement de la communication :**

- Poste_A veut se connecter au Poste_B
- Poste_B génère un message aléatoire, le chiffre avec la clé publique du Poste_A et l'envoi
- Poste_A déchiffre le message avec sa clé privé et le renvoi au Poste_B
- Poste_B le compare avec le message qu'il avait généré avant de le chiffré
- Si c'est le même cela signifie que le Poste_A possède la bonne clé privé et que c'est donc bien lui. 

![[ssh.png|600]]

Pour le RSA prendre RSA 4096 car 2048 trop juste.

**Bonne pratique** = Désactiver la connexion via SSH à root avec un mot de passe. Donc obligation de se connecter via une clé.


#### VNC

Virtual Network Computing (Informatique virtuelle en réseau), est un système de visualisation et de contrôle de l'environnement de bureau d'un ordinateur distant. Il permet au logiciel client VNC de transmettre les informations de saisie du clavier et de la souris à l'ordinateur distant, possédant un logiciel serveur VNC à travers un réseau informatique.

Il existe une version open source [noVNC](https://novnc.com/info.html)

#### RDP

Le RDP (Remote Desktop Protocol) permet d'utiliser un ordinateur de bureau à distance. C'est le plus couramment utilisé. Le protocole RDP a été initialement publié par Microsoft. Il est disponible pour la plupart des systèmes d'exploitation Windows, mais il peut également être utilisé avec les systèmes d'exploitation Mac.

#### HTTP (HTTPS)


### Solutions déploiement systèmes

- Wsus   --> Serveur pour maj
- Sysprep --> image custom pour déploiement (faire poste par poste en physique)
- WDS  /  FOG
- MDT  -> très pertinent
- SCCM (Payant)
- AutoPilot
