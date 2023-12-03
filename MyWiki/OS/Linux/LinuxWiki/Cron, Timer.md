#crontab #cron #timer
## Systemd

Via Systemd, on peut créer des processus et scripts s’exécutant à un moment précis ou intervalle de temps ou via un événement déclencheur.

1. Créer un timer
```bash
sudo mkdir /etc/systemd/system/mytimer.timer.d
sudo vim /etc/systemd/system/mytimer.timer
```
/mytimer.timer
```txt
[Unit]
Description=My Timer

[Timer]
OnBootSec=3min  # si on veut que ça tourne juste après le boot
OnUnitActiveSec=1hour # pour que ça tourne toute les heures

[Install]
WantedBy=timers.target
```

3. Créer un service
```bash
sudo vim /etc/systemd/system/mytimer.service
```
/mytimer.service
```txt
[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target
```

1. Activer le timer
```bash
sudo systemctl daemon-reload
sudo systemctl start mytimer.service
sudo systemctl enable mytimer.service
```

## Cron

Pour automatiser des tâches. Il faut juste créer un script et dire au deamon cron de l'appeler à un temps précis. Se fait dans un fichier **crontab** qu'il faut créer.

|   |   |
|---|---|
|Minutes (0-59)|This specifies in which minute the task should be executed.|
|Hours (0-23)|This specifies in which hour the task should be executed.|
|Days of month (1-31)|This specifies on which day of the month the task should be executed.|
|Months (1-12)|This specifies in which month the task should be executed.|
|Days of the week (0-7)|This specifies on which day of the week the task should be executed.|

Exemple de crontab:
```txt
# Execute scripts
0 0 1 * * /path/to/scripts/run_scripts.sh
```
Tous les premiers jours du mois à minuit. (à 00 minutes et 1 du mois).

**systemctl status service-name**












