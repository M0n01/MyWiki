1) Formater la clé en FAT32
2) Mettre l'iso dedans (CD ou DVD)
3) Utiliser Rufus ou Balena Etcher pour en faire une clé Bootable
4) Si il manque des drivers il faut les récupréer sur internet et les mettre dans le dossier "firmware" de la clé bootable.
5) Brancher la clé, aller dans le BIOS du PC, séléctionner la clé pour démarrer

#### Infos démarrage sur clé (flash)

Si installation sans bootloader, mettre un bootloader sur une autre clé et booter dessus pour lancer l'OS précédemment installer.

##### ==Ubuntu==
Démarrer sur la clé en version de test 
Pour installer l'OS (sur la machine) -> lancer l'installateur
L'installateur Ubiquity permet une installation sans bootloader
```bash
ubiquity -b
```

##### ==Debian==
Mettre une version flash sur la clé
Pour installer l'OS (sur la machine) -> lancer l'installateur
L'installateur Calamares ne permet pas d'installation sans bootloader 

