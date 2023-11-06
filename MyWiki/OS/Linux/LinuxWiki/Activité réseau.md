
## Relever les services réseau

Pour héberger un service réseau -> au moins une adresse IP. Chaque service réseau pourra alors ouvrir un port en écoute et l'associer à cette adresse IP.
- **`ss`** et **`lsof`** : relever les ports en écoute de la machine.

>[!Info]
>La commande **`ss`** (pour "socket statistics") est fournie par le package **`iproute2`** installé en standard sur les distributions.
>La commande **`netstat`**, mais le paquet **`net-tools`**, qui reste disponible, n'est plus maintenu et n'est donc plus installé par défaut sur les distributions.

```bash
ss -lptun
```

>[!Info]
>La commande **`lsof`** (pour "list open files"), fournie par le package **`lsof`** du même nom, s'attache à relever tous les **fichiers ouverts**. Il est donc possible de retrouver ces éléments avec cette commande. (car sur Linux, tout est fichier)

Informations plus compactes:
```bash
lsof -Pi
```
Informations sur un processus en particulier:
```bash
lsof -p 553  # 553 -> PID
```
Filtrer par port:
```bash
lsof -i :22  # 22 -> port ssh
```
Filtrer part fichier (qui ouvre quel fichier):
```bash
lsof /var/log/auth.log  # qui ouvre le fichier auth.log ? rsyslogd 
```

## Observer l’activité réseau sur les interfaces

- **`bmon`** : Bandwidth Monitor. Vise à afficher le trafic en émission et réception sur les cartes. Il s'installe avec le package du même nom **`bmon`**.
- **`iptraf`** : propose l'affichage des connexions de manière plus détaillée, en indiquant notamment les sources ou les destinations relatives au trafic. Il s'installe avec le package du même nom :**`iptraf`**.

quelques outils d'analyse de trafic en temps réel :
- **`Nload`**,
- **`iftop`**,
- **`slurm`**,
- **`tcptrack`**,
- **`speedometer`** (il a l'avantage de présenter, en mode terminal, une vue assez colorée du trafic sur une interface)

## Capturez le trafic réseau d’une interface

- **`tcpdump`** : Capture de trames. Utilitaire en mode terminal. Il s'installe avec le package portant le même nom. Il est très verbeux, il est important de bien spécifier les options de capture en fonction de votre besoin.
- **`wireshark`** : analyseur de trames réseau.

>[!Info]
>Avec **`tcpdump`**, on a aussi la possibilité d’enregistrer notre capture dans un fichier, qui sera ensuite lisible par **[`wireshark`](https://www.wireshark.org/)**.

Informations avec détails des protocoles (trop d'info):
```bash
tcpdump -vv  # mode verbeux
```
Pour ssh:
```bash
tcpdump port 22
```
Pour https, en mettant la capture dans un fichier (qu'on pourra ouvrir avec wireshark):
```bash
tcpdump port 443 -w /home/seb/capture443.dump
```


