
## Serveurs TFTP en tant qu'emplacements de sauvegarde

À mesure que le réseau se développe, les images du logiciel Cisco IOS et les fichiers de configuration peuvent être stockés sur un serveur TFTP central. Cela permet de contrôler le nombre d'images IOS et les révisions correspondantes, ainsi que les fichiers de configuration à gérer.

Quel que soit le réseau, il est recommandé de conserver une copie de sauvegarde de l'image du logiciel Cisco IOS, afin de se prémunir de la dégradation ou de la suppression accidentelle de l'image système du routeur.

Un serveur TFTP permet de télécharger des images logicielles et des configurations par l’intermédiaire du réseau. Le serveur TFTP réseau peut être un autre routeur, une station de travail ou un système hôte.

## Exemple de sauvegarde d'une image IOS sur un serveur TFTP

**Étape 1. Envoyez une requête ping au serveur TFTP.**
```
R1# ping 172.16.1.100
```

**Étape 2. Vérifiez la taille de l'image en flash.**
```
R1# show flash0:
```
Vérifiez que le serveur TFTP possède un espace disque suffisant pour accueillir l'image du logiciel Cisco IOS

**Étape 3. Copiez l'image sur le serveur TFTP.**
```
R1# copy flash: tftp:
Source filename []? ISR4200-UniversalK9_IAS.16.09.04.Spa.bin Address or name of remote host []? 172.16.1.100 
Destination filename [isr4200-universalk9_ias.16.09.04.SPA.bin]? Writing isr4200-universalk9_ias.16.09.04.SPA.bin... !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! (output omitted) 517153193 bytes copied in 863.468 secs (269058 bytes/sec)
```


## Exemple de copie d'une image IOS sur un périphérique

**Étape 1. Envoyez une requête ping au serveur TFTP.**

**Étape 2. Vérifiez la quantité de flash libre.**
```
R1# show flash:
```

**Étape 3. Copiez la nouvelle image de l'IOS en flash.**
```
R1# copy tftp: flash:
Source filename []? ISR4200-UniversalK9_IAS.16.09.04.Spa.bin
```


## La Commande «boot system»

Pour effectuer une mise à niveau à l'image IOS copiée après son enregistrement dans la mémoire Flash du routeur, configurez le routeur de façon à charger la nouvelle image pendant le démarrage à l'aide de la commande **boot system** comme illustré à l'exemple. Enregistrez la configuration. Redémarrez le routeur pour qu'il démarre avec la nouvelle image.

```
R1# configure terminal 
R1(config)# boot system lash0:isr4200-universalk9_ias.16.09.04.SPA.bin
R1(config)# exit
R1# R1# copy running-config startup-config 
R1# R1# reload
Proceed with reload? [confirm] 
```

Au démarrage, le code bootstrap analyse le fichier de configuration initiale dans la mémoire vive non volatile à la recherche des commandes **boot system** qui indiquent le nom et l'emplacement de l'image du logiciel Cisco IOS à charger. Plusieurs commandes **boot system** peuvent être saisies successivement pour créer un plan d'amorçage à tolérance de panne.

En l'absence de commandes **boot system** dans la configuration, le routeur charge par défaut la première image Cisco IOS valide dans la mémoire Flash et l'exécute.

Une fois le routeur est démarré, pour vérifier le chargement de la nouvelle image.
```
R1# show version
System image file is "flash:isr4200-universalk9_ias.16.09.04.SPA.bin"
```

