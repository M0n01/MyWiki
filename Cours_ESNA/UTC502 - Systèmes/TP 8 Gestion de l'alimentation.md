>Gaël le Maréchal

### Manipulations de base :

1. Utilisez la commande uptime pour afficher les informations de disponibilité du système, y compris la durée de fonctionnement et la charge de la CPU
```bash
user@LabUTC502:~/cnam-utc502-os$ uptime
 15:05:14 up 11:54,  1 user,  load average: 0.00, 0.00, 0.00
```

2. Utilisez la commande free pour afficher les informations de mémoire, y compris la mémoire utilisée et disponible
```bash
user@LabUTC502:~/cnam-utc502-os$ free
            total       used        free     shared  buff/cache   available
Mem:       4025928     2466724     264004    45260     1295200     1277384
Swap:      3096568      141924     2954644
```

3. Utilisez la commande df -h pour afficher les informations d'espace disque, y compris l'espace utilisé et disponible sur chaque partition
```bash
user@LabUTC502:~/cnam-utc502-os$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           394M  972K  393M   1% /run
/dev/sda1       100G   12G   84G  12% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
/dev/loop0       56M   56M     0 100% /snap/core18/2538
tmpfs           394M  848K  393M   1% /run/user/1000
/dev/loop2      106M  106M     0 100% /snap/core/16202
/dev/loop1       64M   64M     0 100% /snap/core20/2105
/dev/loop3      2.0M  2.0M     0 100% /snap/btop/655
```

4. Utilisez la commande powertop pour afficher les informations de consommation d'énergie, y compris les processus les plus gourmands en énergie et les suggestions pour améliorer l'efficacité énergétique
![[Pasted image 20231221150723.png]]

5. Utilisez la commande pm-suspend pour mettre en veille le système
```bash
pm-suspend
```

6. Utilisez la commande pm-hibernate pour mettre en hibernation le système
```bash
pm-hibernate
```

7. Utilisez la commande shutdown -h now pour éteindre le système de façon ordonnée
```bash
shutdown -h now
```

8. Elaboration d'un petit programme de contrôle parental maison :

#### Travail à faire :

Créer un programme Python qui permet de gérer l'utilisation de l'ordinateur à la maison en mettant en place des limites de temps et de ressources pour assurer une utilisation saine et responsable.

**Etapes :**

1. Créer un programme qui surveille en continu le débit réseau et la mémoire utilisée par l'ordinateur. Si l'un des deux dépasse un seuil prédéfini, mettre le PC en hibernation automatiquement. Pour surveiller le débit réseau et la mémoire utilisée, vous pouvez utiliser les bibliothèques psutil et shutil.
```python
import psutil
import subprocess
import threading
import schedule

seuil_memoire = 80  # Seuil de mémoire en pourcentage
seuil_debit = 1000000  # Seuil de débit en octets

def surveiller_hiberner(seuil_memoire, seuil_debit):
    while True:
        memory_use = psutil.virtual_memory().percent
        debit_reseau = psutil.net_io_counters().bytes_sent + psutil.net_io_counters().bytes_recv

        if memory_use > seuil_memoire or debit_reseau > seuil_debit:
            subprocess.run(["pm-hibernate"])
        time.sleep(60)

# Surveillance en arrière-plan
surveillance_thread = threading.Thread(target=surveiller_hiberner, args=(seuil_memoire, seuil_debit))
surveillance_thread.start()
```

2. Ajouter une fonctionnalité qui arrête automatiquement l'ordinateur après un temps de fonctionnement de 30 minutes (bibliothèque time).
```python
import time

def arret_automatique():
    time.sleep(1800)
    subprocess.run(["shutdown", "-h", "now"])

arret_thread = threading.Thread(target=arret_automatique)
arret_thread.start()
```

3. Planifier le démarrage automatique d’un ordinateur sous Linux à une heure spécifique pour faire ses devoirs. (bibilothèques schedule et subprocess)
```python
import time
import subprocess
import schedule

def tache_planifiee():
    print("Il est l'heure de faire les devoirs !")

def start_program():
    # Tâche planifiée
    schedule.every().day.at("17:00").do(tache_planifiee)
Il est temps
    # Boucle principale
    while True:
        schedule.run_pending()
        time.sleep(1)

start_program()
```

Programme complet :
```python
import psutil
import subprocess
import threading
import schedule
import time

seuil_memoire = 80  # Seuil de mémoire en pourcentage
seuil_debit = 1000000  # Seuil de débit en octets

def surveiller_hiberner(seuil_memoire, seuil_debit):
    while True:
        memory_use = psutil.virtual_memory().percent
        debit_reseau = psutil.net_io_counters().bytes_sent + psutil.net_io_counters().bytes_recv

        if memory_use > seuil_memoire or debit_reseau > seuil_debit:
            subprocess.run(["pm-hibernate"])
        time.sleep(60)

# Surveillance en arrière-plan
surveillance_thread = threading.Thread(target=surveiller_hiberner, args=(seuil_memoire, seuil_debit))
surveillance_thread.start()

def arret_automatique():
    time.sleep(1800)
    subprocess.run(["shutdown", "-h", "now"])

arret_thread = threading.Thread(target=arret_automatique)
arret_thread.start()

def tache_planifiee():
    print("Il est l'heure de faire les devoirs !")

def start_program():
    # Ajouter la tâche planifiée
    schedule.every().day.at("17:00").do(tache_planifiee)
Il est temps
    # Boucle principale
    while True:
        schedule.run_pending()
        time.sleep(1)

start_program()
```