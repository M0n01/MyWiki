## Type de chiffrement 

- ==**Symétrique**== : La même clé pour chiffrer et déchiffrer.
	- Exemple : mdp pour ouvrir un zip ou pour se connecter à la Wifi

**==A la fin, toute les transmission Wifi, VPN... TOUT utilisent le chiffrement symétrique car c'est le plus rapide.==** Le téléphone Rouge Washington et Moscou : Symétrique.

- ==**Asymétrique**== : Utiliser pour échanger les clés symétriques. Si on chiffre avec une clé (publique ou privé) on peu déchiffrer avec l'autre (publique ou privé).

- ==**Signature électronique**== : Hash + Chiffrement. On chiffre le Hash d'un fichier mais pas tout le fichier car trop lourd. Le récepteur va hasher le fichier et le comparer au hash qu'il a déchiffrer, si ce n'est pas le même c'est soit que le fichier à été modifier soit qu'on a pas la bonne clé. C'est pour s'assurer de l'identité de l'émetteur.

- ==**Certificat**== : 
	- Identité : DNS (nom du site) 
	- Clé publique du site 
	- Nom de l'autorité de certification
	- Signature de l'autorité de certification (hash du fichier chiffré par l'entité de certif) + Date + nom de l'action


### Chiffrement salé d'un mot de passe (hash)

Pour rendre les attaques par dictionnaire inefficace.

Par exemple : $1$7RBNnEK2 $PRFufDu3WwXg72Ay2Moci0

Le hash est composé de trois éléments, séparés par le symbole $ :

1.  L'identificateur de type de hash : Ici "1". Cela indique que le hash a été généré en utilisant l'algorithme MD5. "6" correspond au SHA-512 etc.

2.  La valeur de sel : Ici "7RBNnEK2". Le sel est utilisé pour ajouter de la complexité au hash et le rendre plus difficile à craquer. Lorsqu'un mot de passe est hashé, la valeur de sel est combinée avec le mot de passe avant que le hash ne soit généré.

3.  Le mot de passe hashé : Ici "PRFufDu3WwXg72Ay2Moci0". C'est le résultat de l'application de l'algorithme MD5 à la combinaison du mot de passe et du sel.