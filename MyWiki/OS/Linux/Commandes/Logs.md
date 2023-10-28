
**==syslog==** : Outil de journalisation simple mais largement utilisé.

**==syslog-ng==** : Version plus avancée avec des fonctionnalités supplémentaires.

**==journalctl==** : Outil de journalisation plus récent et plus structuré conçu pour les distributions Linux basées sur systemd.

## journalctl

Afficher log enregistrés depuis le dernier redémarrage du système.
```bash
journalctl
```

Affiche log en temps réel
```bash
journalctl -f 
```

Affiche log associés à un service spécifique.
```bash
journalctl -u nom_service
```
 
Affiche log enregistrés entre deux dates et heures spécifiées.
```bash
journalctl --since "yyyy-mm-dd HH:MM:SS" --until "yyyy-mm-dd HH:MM:SS"
```

Affiche log du noyau (kernel) enregistrés depuis le dernier redémarrage.
```bash
journalctl -k
```
