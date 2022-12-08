![[logoWindows.png|500]]

# Commandes

### SSH

Exemple, une machine windows se connecte en ssh à une machine linux :
- Dans PuttyGen:
	- Générer une clé privé 
	- Charger la clé privé
	- Récupérer la clé publqiue correspondante

Copier la clé publique et la coller dans authorized_keys. Voir  [[Sys linux]]

- Dans Putty:
	- Session > rentrer ip de la machine cible
	- SSH > Auth > Browse > prendre la clé privé
	- Open
