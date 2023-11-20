
![[Pasted image 20231113093134.png]]

## Revoir

Couche Physique
Sous réseaux
Plan avec les 5 routeurs (4_IP)
Faire TD : 5_Transport
Voir une vidéo sur DNS


### Objectifs

- Comprendre la couche 1
	- Code (NRZ, Manchester, Manchester différentiel, Miller)
	- Définitions
	- Calculs 
	- En gros refaire fiche exercice
- Eval : Basé sur exos fait en cours

## Couche 1 : Physique

Conversion signal numérique en signal analogique -> Transformé de Fourrier
Le codage NRZ ne subit pas d'encodage

Porteuse -> signal analogique de base

**Définitions** : Bande Passante, Valence d'une voie, Moment élémentaire, Vitesse de modulation, Débit binaire.

Pour calculer débit -> valence et vitesse de modulation

Encodage Manchester -> Ethernet

4 bits par signal = valence de 16.


## Réseau local


|         |                                 |                                     |               |
| ------- | ------------------------------- | ----------------------------------- | ------------- |
| Class A | 1.0.0.0 to  <br>127.0.0.0       | 10.0.0.0 to  <br>10.255.255.255     | 255.0.0.0     |
| Class B | 128.0.0.0 to  <br>191.255.0.0   | 172.16.0.0 to  <br>172.31.255.255   | 255.255.0.0   |
| Class C | 192.0.0.0 to  <br>223.255.255.0 | 192.168.0.0 to  <br>192.168.255.255 | 255.255.255.0 |

