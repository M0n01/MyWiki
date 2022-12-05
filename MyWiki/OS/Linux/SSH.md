**Sur RaspberryPi:**
Autoriser les connexions SSH
```bash
sudo raspi-config
```
- interfaces options
- enable ssh

**Sur les autres machines:**
Connexion SSH normal
```bash
ssh pi@addresse_ip_raspberry
```
Connexion SSH permettant d'ouvrir des interfaces graphiques
```bash
ssh -X pi@addresse_ip_raspberry
```
- Entrer le mot de passe de la RPi