1. Lors du boot, appuyer sur ==e== lorsque le menu Grub s'affiche afin d'éditer les paramètres de démarrage
2. Ajouter ==init=/bin/bash== à la fin de la ligne terminant par ==ro quiet== afin de démarrer directement en mono-utilisateur sur le programme bash
3. Une fois Linux démarré, remonter ==/== en lecture/écriture avec ==mount -n -o remount,rw /==
4. Modifier le mot de passe root avec la commande ==passwd==







