
Fait sur avec Firefox

Problème :
- Lorsque l'on clique sur la page on a l'erreur "Wrong user-agent: you are not the "admin" browser!"

Solution :
- Inspecter la page
- Aller dans "Réseau"
- Recharger la page
- Sélectionner la requête "GET" avec comme fichier "/web-serveur/ch2/"
- En haut à droite cliquer sur "Renvoyer"
> Une colonne apparaît à gauche
- Modifier le "User-Agent" en "admin"
- Cliquer sur "Envoyer"
- Aller dans "Réponse" à droite
- Recup le flag

