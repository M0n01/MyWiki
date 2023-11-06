![[TUX.png|500]]
# Raccourci clavier

CTRL + alt + t   // ouvre un terminal
CTRL + r    // rechercher dans l’historique de commande (dans terminal). Si on refait CTRL + r pour remonter dans les occurrences.
CTRL + a    // revenir au début de la ligne
CTRL + e    // revenir à la fin de la ligne
CTRL + k    // supprime la ligne

# Astuces et Bonnes Pratiques

- Ajouter « | grep mot »  après une commande pour trouver un mot. Le «  |  » permet de concaténer plusieurs commande (passer le résultat d’une commande à une autre).
- Quand on  ne connait pas la taille du fichier -> cat, grep, less... mais pas nano ou vim. Pour fichier compressé (gzip) -> zcat, zgrep, zless

# Commandes

### Orientation
```bash
history    // affiche historique de commande.
	!numéro   // pour rappeler la commande

find /* -name nom_dossier_ou_ficher   // localiser un élément depuis la racine

ls -l      // liste en mode détaillé (droits etc)
ls -l *.conf | sort    // liste et trie avec détails les fichiers .conf
ls -lah                // liste et trie en plus lisible
ls -a         // liste les fichiers cachés
ls -la        // affiche fichier caché
dump      // info au format hexa
tail    // juste les dernières lignes

who     // liste des terminaux ouverts

users   // liste des utilisateurs connectés

ps -ef     // liste des processus

ps uww num_PID    // infos sur un processus

top    // info consommation ressources

kill // stoper un process
kill -9 // forcer

sudo -H -i    // être comme root mais sur un utilisateur lambda

grep   // permet de filtrer le résultat de la commande

commande >> fichier   // ajouter résultat dans un fichier (écrase pas)
commande > fichier    // ajouter résultat dans un fichier (écrase)

apt purge mon_paquet    // en cas de problèmes de installation

env | grep proxy // vérifier proxy

cp -R <source_folder> <destination_folder> // copier dossier

history  // voir historique

sudo apt-cache search nom_commande  // obtenir le nom du package

grep xitix /etc/password  // afficher le shell utilisé par le compte
```

Modifier le fichier /etc/passwd et indiquer un interpréteur de commandes comme /usr/bin/nologin ou /dev/zero ou encore /dev/null, garantit que l'utilisateur ne pourra jamais lancer de shell à la suite du processus d'authentification lors de la connexion. Et donc, il ne pourra pas faire grand chose… .

- Voir DAC et MAC linux (droits)
### Metadata image
```bash
exiftool image.png
```


## Non-free and contrib source for debian 12

```bash
deb http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware
debrsrc http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware

deb http://deb.debian.org/debian-security/ bookworm-security main contrib non-free non-free-firmware
debrsrc http://deb.debian.org/debian-security/ bookworm-security main contrib non-free non-free-firmware

deb http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware
debrsrc http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware
```

