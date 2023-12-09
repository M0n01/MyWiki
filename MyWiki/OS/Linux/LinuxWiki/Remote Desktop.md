#X11 #VNC 
### X11

Le X11 est un système fixe constitué d'un ensemble de protocoles et d'applications qui nous permettent d'appeler des fenêtres d'application sur des écrans dans une interface utilisateur graphique. X11 est prédominant sur les systèmes Unix, mais les serveurs X sont également disponibles pour d'autres systèmes d'exploitation.

L'inconvénient majeur du X11 est la transmission de données non cryptées. Cependant, cela peut être surmonté en tunnelant le protocole SSH.

Pour cela, nous devons autoriser le transfert X11 dans le fichier de configuration SSH (/etc/ssh/sshd_config) sur le serveur qui fournit l'application en changeant cette option par yes.

```bash
cat /etc/ssh/sshd_config | grep X11Forwarding

X11Forwarding yes
```

On peut ensuite lancer une application depuis notre client avec l'option -X
```bash
ssh -X alex@10.129.23.11 /usr/bin/firefox
```

### VNC

Traditionnellement, le serveur VNC écoute sur le port TCP 5900. Il y propose donc son affichage 0. D'autres écrans peuvent être proposés via des ports supplémentaires, principalement 590[x], où x est le numéro d'affichage. L'ajout de plusieurs connexions serait attribué à un port TCP supérieur comme 5901, 5902, 5903, etc.

Pour ces connexions VNC, de nombreux outils différents sont utilisés. Parmi eux figurent par exemple :

- TigreVNC
- SerréVNC
- RéelVNC
- UltraVNC

Les outils les plus utilisés pour ce type de connexions sont UltraVNC et RealVNC en raison de leur cryptage et de leur sécurité supérieure.