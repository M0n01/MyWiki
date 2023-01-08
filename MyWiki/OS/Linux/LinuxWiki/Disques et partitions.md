## Volumes logiques

![[LVM.PNG]]


==**Système de fichier**==: Permet de décoder les données et les présenter sous forme de répertoires.

#### LVM 

>Gestionnaire de volumes logiques (_Logical Volume Manager_), permettant de structurer les partitions de nos systèmes en ensembles et sous-ensembles logiques. Permet d'augmenter la taille des partitions à chaud. Le LVM est à la fois une méthode de gestion des partitions des systèmes types Linux ou Unix. Mais, c’est également un logiciel de gestion d’utilisation des espaces de stockage d’un ordinateur.

#### PV

>Physical Volume (disque/partition). Ne peut contenir que 1 VG. Le plus souvent on parle de partition, car un disque (comme /dev/sdb ou /dev/mpatha) doit contenir au moins une partition, nommée alors /dev/sdb1.

#### VG

>Virtual Group. Contient des LV et peut être sur plusieur PV.

#### LV

>Logical Volume. Il s’agit d’un espace défini dans le groupe de volumes que l’on délimite pour y stocker un système de fichiers. On peut se représenter cela comme l’équivalent des partitions physiques, à l’exception près que celles-ci sont alors redimensionnables facilement par ajout de nouveaux volumes physiques. Un volume logique doit, théoriquement contenir un point de montage, pour pouvoir accrocher le système de fichiers à présenter aux utilisateurs.
