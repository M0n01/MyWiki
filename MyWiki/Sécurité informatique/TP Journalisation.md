
## TP3a-ntp-rsyslog

### Client NTP

```bash
nano /etc/systemd/timesyncd.conf

NTP = 192.168.2.3  # décommenter et ajouter IP
```

Puis
```bash
systemctl daemon-reload
timedatectl set-ntp 1
systemctl restart systemd-timesyncd
systemctl status systemd-timesyncd
timedatectl
```

### Serveur NTP

Installer le package ntp
```bash
apt install ntp
```

Modifier la config ntp :
```bash
nano /etc/ntp.conf

listen on 192.168.5.78 # service accessible sur (interface interne)
fudge 127.127.1.0 stratum 10 # pour créer sa référence de temps
server 127.127.1.0 # synchro sur soi-même
restrict 192.168.2.0 mask 255.255.243.0 notrust # accès depuis le lan
```

Relais ntp (optionnel) :
```bash
server ntp.rtprive
server 192.168.2.1
```

Tester le service :
```
service ntp restart
ntpq -p
```

### Gestion des Logs

Configuration du serveur rsyslog sous linux

Sauvegarder le fichier /etc/rsyslog.conf dans /etc/rsyslog.conf.old :
```bash
cp /etc/rsyslog.conf /etc/rsyslog.conf.old
```

Modifier la configuration pour que les messages local3.* se retrouvent dans /var/log/local3.log ex :
```bash
nano /etc/rsyslog.conf

local3.*                    -/var/log/local3.log
local3.*                    @@192.168.5.65
```

Redémarrer le serveur syslog :
```bash
service rsyslog restart
```

Tester le serveur en utilisant la commande : 
```bash
logger -p local3.err "message test erreur local3"
```

### Accepter/Ecouter les logs des postes du réseau

modifier la configuration du script de démarrage de syslog (/etc/rc.d/initd/rsyslog) afin qu'il démarre avec les options udp et tcp :
```bash
nano /etc/rsyslog

#provides UDP syslog reception
module(load="imudp")                    # décommenter
input(type="imudp" port="514")      # décommenter
#provides TCP syslog reception
module(load="imtcp")                    # décommenter
input(type="imtcp" port="514")      # décommenter
```  

Redémarrer le service syslog :
```bash
service rsyslog restart
```

>[!Remarque]
>vous pouvez videz les fichiers logs avec la commande :
>echo > /var/log/syslog

### Surveiller un poste

Sauvegarder le fichier /etc/syslog.conf dans /etc/rsyslog.conf.old : 
```bash
cp /etc/rsyslog.conf /etc/rsyslog.conf.old
```

Modifier la configuration pour que les messages local3.* soient envoyées à votre serveur :
@ip:port en UDP ou @@ip:port en tcp
```bash
nano /etc/rsyslog.conf

#local3.*                   /var/log/local3.log    // commenter 
local3.*                    @@192.168.5.51    # mettre IP serv (mon IP si je suis serv)

```

Redémarrer le service syslog :
```bash
service rsyslog restart
```

Tester le serveur en utilisant la commande : 
```bash
logger -p local3.err "message test erreur local3"
cat /var/log/local3.log
```


## TP3b-syslog-ng splunk

