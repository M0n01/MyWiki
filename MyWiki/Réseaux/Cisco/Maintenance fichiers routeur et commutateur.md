il existe plusieurs façons de copier ou de mettre à jour vos configurations, puis de les coller simplement. Pour ce faire, vous devrez savoir comment afficher et gérer vos systèmes de fichiers.

Le Cisco IFS (IOS File System) permet à l'administrateur de naviguer dans différents répertoires et d'établir la liste des fichiers d'un répertoire. L'administrateur peut également créer des sous-répertoires en mémoire flash ou sur un disque. Les répertoires disponibles dépendent du périphérique.

Affiche tous les systèmes de fichiers disponibles sur un routeur/commutateur
```
Router# show file systems
```

Cette commande fournit des informations utiles telles que la quantité de mémoire totale et libre, le type de système de fichiers, et ses autorisations. Les autorisations comprennent lecture seule (ro), l'écriture seule (wo), et la lecture et l'écriture (rw). 

Bien que plusieurs systèmes de fichiers soient répertoriés, seuls les systèmes de fichiers TFTP, Flash et NVRAM nous intéressent.

>[!Remarque]
>L'astérisque devant le système de fichiers Flash. Il indique qu'il s'agit du système de fichiers par défaut actuel. L'IOS amorçable se trouve dans la mémoire Flash. Par conséquent, le symbole dièse (#) est ajouté à la liste Flash pour indiquer qu'il s'agit d'un disque amorçable.

**système de fichiers Flash**
```
Router# dir
```

**système de fichiers NVRAM**
```
Router# cd nvram: 
Router# pwd 
nvram:/ 
Router# dir
```


## Utiliser un fichier texte pour sauvegarder une configuration

Les fichiers de configuration peuvent être enregistrés dans un fichier texte en utilisant Tera Term.

- **Étape 1**. Dans le menu File (Fichier), cliquez sur **Log**.  

- **Étape 2**. Choisissez l'emplacement où vous souhaitez enregistrer le fichier. Tera Term commence à capturer le texte.  

- **Étape 3**. Après avoir démarré la capture, exécutez la commande **show running-config** ou **show startup-config** à l'invite du mode d'exécution privilégié. Le texte affiché dans la fenêtre du terminal est alors placé dans le fichier choisi.  

- **Étape 4**. Lorsque la capture est terminée, sélectionnez **Close(Fermer)** dans le Tera Term : Fenêtre de journal Fenêtre du journal. 

- **Étape 5**. Affichez le fichier pour vérifier qu'aucun problème n'est survenu.


## Utiliser un fichier texte pour restaurer une configuration

Une configuration peut être copiée à partir d'un fichier et ensuite directement collée sur un périphérique. L'IOS exécute chaque ligne du texte de configuration en tant que commande. Cela signifie que le fichier devra être modifié pour garantir que les mots de passe cryptés sont en texte clair, et que les textes ne correspondant pas à des commandes, par exemple **--More--** ,et les messages IOS sont supprimés. En outre, vous pouvez ajouter **enable** et **configure terminal** au début du fichier ou entrer en mode de configuration globale avant de coller la configuration.

Au lieu de copier et coller, une configuration peut être restaurée à partir d'un fichier texte à l'aide de Tera Term

En utilisant Tera Term, la procédure est la suivante:

- **Étape 1**. Dans le menu File (Fichier), cliquez sur **Send** file. 

- **Étape 2**. Recherchez le fichier à copier sur le périphérique et cliquez sur **Open**

- **Étape 3**. Tera Term colle alors le fichier dans le périphérique.


## Utiliser TFTP pour sauvegarder et restaurer la configuration


#### Utiliser TFTP pour sauvegarder la configuration

Les copies des fichiers de configuration doivent être stockées en tant que fichiers de sauvegarde afin de parer à toute éventualité. Les fichiers de configuration peuvent être stockés sur un serveur TFTP (Trivial File Transfer Protocol) ou sur un périphérique de stockage USB. Vous devez également inclure un fichier de configuration dans la documentation du réseau.

Enregistrer la configuration en cours ou la configuration de démarrage sur un serveur TFTP
```
R1# copy running-config tftp 
Remote host []?192.168.10.254 
Name of the configuration file to write[R1-config]? R1-Jan-2019
Write file R1-Jan-2019 to 192.168.10.254? [confirm]
Writing R1-Jan-2019 !!!!!! [OK]
```

Procédez comme suit pour sauvegarder la configuration en cours sur un serveur TFTP:

- **Étape 1**. Saisissez la commande **copy running-config tftp**

- **Étape 2**. Entrez l'adresse IP de l'hôte sur lequel le fichier de configuration sera stocké.  

- **Étape 3**. Entrez le nom à attribuer au fichier de configuration. 

- **Étape 4**. Appuyez sur Entrée pour confirmer chaque choix.

#### Utiliser TFTP pour restaurer la configuration

Restaurer la configuration en cours ou la configuration de démarrage à partir d'un serveur TFTP
```
copy tftp running-config ou copy tftp startup-config
```

- **Étape 1**. Saisissez la commande **copy tftp running-config** 

- **Étape 2**. Saisissez l'adresse IP de l'hôte sur lequel le fichier de configuration est stocké.

- **Étape 3**. Entrez le nom à attribuer au fichier de configuration.  

- **Étape 4**. Appuyez sur **Enter** pour confirmer chaque choix.


## Des ports USB d'un routeur Cisco

Afficher le contenu du disque Flash USB
```
Router# dir usbflash0:
```

Copier le fichier de configuration sur le disque Flash USB
```
R1# copy running-config usbflash0:
```

Afficher le fichier sur le disque USB
```
R1# dir usbflash0:/
```

Afficher le contenu
```
R1# more usbflash0:/R1-Config
```

#### Restaurer les configurations avec un disque Flash USB

Pour copier le fichier, il est nécessaire de modifier le fichier USB R1-Config au moyen d'un éditeur de texte. En supposant que le nom du fichier est **R1-Config**, utilisez la commande **copy usbflash0:/R1-Config** _running-config_ pour restaurer une configuration en cours.


## Procédures de récupération des mots de passe

Les mots de passe chiffrés, tels que les mots de passe secrets chiffrés, doivent être remplacés après la récupération. Selon l'appareil, la procédure détaillée de récupération de mot de passe varie. Cependant, toutes les procédures de récupération des mots de passe pour les périphériques Cisco suivent le même principe:

**Étape 1**. Activez le mode ROMMON.  
**Étape 2**. Modifiez le registre de configuration.  
**Étape 3**. Copiez la configuration de démarrage dans la configuration d'exécution.  
**Étape 4**. changer le mot de passe.  
**Étape 5**. Enregistrez la configuration en cours d'exécution comme nouvelle configuration de démarrage.  
**Étape 6**. Rechargez l'appareil.

L'accès de la console au périphérique par le biais d'un terminal ou d'un émulateur de terminal sur PC est requis pour la récupération de mots de passe. Les paramètres du terminal pour accéder au périphérique sont les suivants:

- Débit de 9600 bauds
- Aucune parité
- 8 bits de données
- 1 bit d'arrêt
- Aucun contrôle de flux