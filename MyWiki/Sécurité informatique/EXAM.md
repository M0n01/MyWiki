
## Clés de chiffrement

- MD5 = Message Digest algorithm
	- Cassé
- SHA = Secure Hash Algorithm 
	- SHA-1 -> Cassé
	- SHA-256 -> Sûr

- ACL = liste de permissions
	cat /etc/group      // affiche la liste des groupes
	cat /etc/passwd     // affiche la liste des utilisateurs
	cat /etc/shadow     // affiche hash de mdp

- Serveur authentification ntlm



- HTTPS
	- HTTPS utilise le port 443 en TCP


- **==Firewall ==**: Ne s'occupe que des couches Physique, Liaison de données, Réseau et Transport.
- **==Proxy (ou Relais)==** : S'occupe des couches au dela de la couche Transport. Pour les paquets sortant du réseau.
- ==**Reverse Proxy**== : Pareil mais pour les paquets entrant dans le réseau.