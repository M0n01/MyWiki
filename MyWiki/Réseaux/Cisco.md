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
ou
- Ctrl+Maj+9

## Commandes

initial configuration dialog ? NO

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

>Les lignes VTY (terminal virtuel) activent l'accès à distance au périphérique en utilisant Telnet ou SSH.

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

>Les fichiers startup-config et running-config affichent la plupart des mots de passe en clair. C'est une menace à la sécurité dans la mesure où n'importe quel utilisateur peut voir les mots de passe utilisés s'il a accès à ces fichiers.

Chiffrer tous les mots de passe non chiffrés.
```
Sw-Floor-1# configure terminal

Sw-Floor-1(config)# service password-encryption
```

Pour vérifier que les mdp sont bien chiffrés
```
Sw-Floor-1(config)# show running-config
```

### RIP

>Regarde le chemin le plus **court** par le nombre de routeur à passer.
>Ne prend pas en compte la bande passante des liaisons.


### OSPF

>Regard le chemin le plus **rapide** par la bandes passante des liaisons.

voir [Info OSPF](https://cisco.goffinet.org/ccna/ospf/messages-ospf/)

1) Définir IP sur interfaces
2) Mettre en place OSPF
##### Config OSPF
```
router(config)#router ospf 1  
router(config-router)#network ip_voisin1 masque_inversé area 0  
router(config-router)#network ip_voisin2 masque_inversé area 0  
router(config-router)#end
```

##### Config Loopback du routeur
```
Router0B(config)# interface loopback 0
Router0B(config-if)# ip address adresse_IP 255.255.255.255
```
- Mémoriser la conf OSPF
- Supprimer la conf OSPF
```
Router0B(config)# no router ospf 1
```
- Le réinjecter
```
router(config)#router ospf 1  
router(config-router)#network ip_voisin1 masque_inversé area 0  
router(config-router)#network ip_voisin2 masque_inversé area 0  
router(config-router)#end
```

##### Informations
Afficher les voisins d'un routeur
```
Router0B#show ip ospf neighbor
ou
Router0B#show ip ospf neighbor detail
```

Afficher métrique d'interface
```
Router0B#show ip ospf interface
```

Afficher toute les routes connu et leur Area
```
Router0B#show ip ospf database
```

Afficher informations sur les routes
```
Router0B#show ip route
```

Afficher ID du routeur
```
Router0B#show ip protocols
```

##### Redistribution de route par défaut

```
router(config)#router ospf 1
router(config-router)#default-information originate
```

##### Définir coût d'un lien

```
router(config-if)# bandwidth 64 // spécifie une bande passe en KBit/S 
ou
router(config-if)# ip ospf cost 1562
```

>Pour tenir compte de la métrique des liens gigabits, on doit modifier la base de calcul des coûts ospf. Par defaut, Reference bandwidth est 100 Mbps.
```
router(config)#router ospf 1
router(config-router)#Auto-cost reference bandwidth 1000
```

##### Reset d'un routeur

```
router(config)# config-register 0x2102
router(config)# end
router# write erase
router# reload
Save?[yes/no]: n
reload?[confirm]
```
