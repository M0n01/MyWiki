## Type de chiffrement 

- **Symétrique** : La même clé pour chiffrer et déchiffrer.
	- Exemple : mdp pour ouvrir un zip ou pour se connecter à la Wifi

**==A la fin, toute les transmission Wifi, VPN... TOUT utilisent le chiffrement symétrique car c'est le plus rapide.==** Le téléphone Rouge Washington et Moscou : Symétrique.

- **Asymétrique** : Utiliser pour échanger les clés symétriques. Si on chiffre avec une clé (publique ou privé) on peu déchiffrer avec l'autre (publique ou privé).

- **Signature électronique** : Hash + Chiffrement. On chiffre le Hash d'un fichier mais pas tout le fichier car trop lourd. Le récepteur va hasher le fichier et le comparer au hash qu'il a déchiffrer, si ce n'est pas le même c'est soit que le fichier à été modifier soit qu'on a pas la bonne clé. C'est pour s'assurer de l'identité de l'émetteur.