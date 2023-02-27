![[TUX.png|500]]
# Raccourci clavier

CTRL + alt + t   // ouvre un terminal
CTRL + r    // rechercher dans l’historique de commande (dans terminal)
CTRL + a    // revenir au début de la ligne
CTRL + e    // revenir à la fin de la ligne
CTRL + k    // supprime la ligne

# Astuces et Bonnes Pratiques

- Ajouter « | grep mot »  après une commande pour trouver un mot. Le «  |  » permet de concaténer plusieurs commande (passer le résultat d’une commande à une autre).
- Quand on  ne connait pas la taille du fichier -> cat, grep, less... mais pas nano ou vim. Pour fichier compressé (gzip) -> zcat, zgrep, zless

# Commandes

### Orientation
```bash
history    // affiche historique de commande

ls -l      // liste en mode détaillé (droits etc)
ls -l *.conf | sort    // liste et trie avec détails les fichiers .conf
ls -lah                // liste et trie en plus lisible
ls -a         // liste les fichiers cachés
ls -la        // affiche fichier caché
dump      // info au format hexa
tail    // juste les dernières lignes

who     // liste des terminaux ouverts

users   // liste des utilisateurs connectés

ps     // liste des processus

ps uww num_PID    // infos sur un processus

top    // info consommation ressources

sudo -H -i    // être comme root mais sur un utilisateur lambda

grep   // permet de filtrer le résultat de la commande

commande >> fichier   // ajouter résultat dans un fichier (écrase pas)
commande > fichier    // ajouter résultat dans un fichier (écrase)

apt purge mon_paquet    // en cas de problèmes de installation

env | grep proxy // vérifier proxy

cp -R <source_folder> <destination_folder> // copier dossier

voir LOG JOURNALCTL
et syslog-ng
```

### Metadata image
```bash
exiftool image.png
```
