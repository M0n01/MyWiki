voir [[RéseauxWiki#SSH|SSH]]

### Connexion SSH classique

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
ssh utilisateur@addresse_ip
```
- Exemple:
```bash
ssh pi@addresse_ip_raspberry
```
Connexion SSH permettant d'ouvrir des interfaces graphiques
```bash
ssh -X pi@addresse_ip_raspberry
```
- Entrer le mot de passe de la RPi

Faire un audit ssh de la machine (à revoir)
- apt install ssh-audit
```bash
ssh-audit adresse_IP
```

### Connexion SSH par clés

- Créer une paire de clé rsa 4096
```bash
ssh-keygen -t rsa -b 4096
ls -lh .ssh/     '// endroit où sont stockés les clés'
```
- Autorisé ou non des clés publiques
```bash
touch .ssh/authorized_keys   '// pour mettre les clés publiques'
cat .ssh/id_rsa.pub >> .ssh/authorized_keys   '// rangement'
```
Mettre les clés publiques des autres postes autorisé à se connecter en SSH à notre machine dans authorized_keys.

- Mettre les droits
```bash
chmod 700 .ssh/
chmod 600 .ssh/authorized_keys
chmod 600 .ssh/id_rsa.pub
chmod 600 .ssh/id_rsa
```
- Dans /etc/ssh/sshd_config (Décommenter, voir ajouter si inexistant)
```bash
Port 22
HostKey /etc/ssh/ssh_host_rsa_key
PermitRootLogin no         '// désactiver accès direct en root'
PubkeyAuthentication yes
AuthorizedKeysFile ....
PasswordAuthentication no  '// désactiver authentification par mdp'
ChallengResponseAuthentication no
```
- Restart
```bash
systemctl restart sshd
```

