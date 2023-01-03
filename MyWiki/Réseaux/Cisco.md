## Syntaxe des commandes

Le texte en **gras** signale les commandes et mots-clés à saisir tels quels.

Le texte en *italique* signale les arguments pour lesquels des valeurs doivent être saisies.

Les crochets signalent un élément facultatif (mot-clé ou argument).
```
[x] 
```
  

Les accolades signalent un élément requis (mot-clé ou argument).
```
{x} 
```
  
Les accolades et les lignes verticales encadrées par des crochets signalent un choix obligatoire, au sein d'un élément facultatif. Les espaces sont utilisés pour délimiter clairement les parties de la commande.
```
[x {y | z }] 
```

Info sur une commande
```
description
```

Aide contextuelle (peut aussi se mettre à la fin d'une commande quand on est pas sur de la suite)
```
?
```

### Raccourcis clavier

Effacer tous les caractères à partir du curseur jusqu'à la fin de la ligne de commande
- Ctrl+K 

Effacer tous les caractères à partir du curseur jusqu'au début de la ligne de commande.
- Ctrl+U ou Ctrl+X

Déplace le curseur vers le début de la ligne
- Ctrl+A

Déplace le curseur vers la fin de la ligne
- Ctrl+E

Séquence d'interruption permettant d'abandonner les recherches DNS, traceroutes, pings, etc
- Ctrl+Maj+6

## Commandes

### Nom de périphériques

*Débutent par une lettre;
Ne contiennent pas d'espaces;
Se terminent par une lettre ou un chiffre;
Ne comportent que des lettres, des chiffres et des tirets;
Comportent moins de 64 caractères.*

Changer le nom
```
hostname nouveau_nom
```

Revenir au nom par défaut
```
no hostname
```

### Sécuriser l'accès en mode d'exécution utilisateur (quand on rentre dans le terminal)

Spécifier l'interface
```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# line console 0
```
- Le zéro sert à représenter la première (et le plus souvent, la seule) interface de console.

Spécifier mot de passe
```
Sw-Floor-1(config-line)# password motdepasse
```

Activer accès
```
Sw-Floor-1(config-line)# login
Sw-Floor-1(config-line)# end
```
- La console d'accès requiert à présent un mot de passe avant d'accéder au mode d'exécution utilisateur.

### Sécuriser l'accès en mode d'exécution (enable, le plus important)

```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# enable secret motdepasse    // Spécifie le mot de passe
  
Sw-Floor-1(config)# exit
```

### Sécuriser l'accès aux lignes VTY

Les lignes VTY (terminal virtuel) activent l'accès à distance au périphérique en utilisant Telnet ou SSH.

Spécifier les lignes VTY, il y en a 16 (de 0 à 15)
```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# line vty 0 15 // Pour les lignes VTY de 0 à 15
```

Spécifier mdp
```
Sw-Floor-1(config-line)# password motdepasse
```

Activer accès VTY
```
Sw-Floor-1(config-line)# login 
Sw-Floor-1(config-line)# end
```

### Chiffrer les mots de passes

Les fichiers startup-config et running-config affichent la plupart des mots de passe en clair. C'est une menace à la sécurité dans la mesure où n'importe quel utilisateur peut voir les mots de passe utilisés s'il a accès à ces fichiers.

Chiffrer tous les mots de passe non chiffrés.
```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# service password-encryption
```

Pour vérifier que les mdp sont bien chiffrés
```
Sw-Floor-1(config)# show running-config
```