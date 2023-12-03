#backup #restore

Exemple d'outils:
- Rsync
- Deja Dup
- Duplicity

C'est bien de chiffrer ses backups.

Exemple d'outils:
- GnuPG
- eCryptfs
- LUKS

Installer Rsync
```bash
sudo apt install rsync -y
```

Backup vers un serveur
```bash
rsync -av /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

Pareil mais avec de la compression et de l’incrémental 
```bash
rsync -avz --backup --backup-dir=/path/to/backup/folder --delete /path/to/mydirectory user@backup_server:/path/to/backup/directory
```
L'option `--delete` supprime les fichiers de l'hôte distant qui ne sont plus présents dans le répertoire source.

Restaurer
```bash
rsync -av user@remote_host:/path/to/backup/directory /path/to/mydirectory
```

Transfert sécuriser de la backup
```bash
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```
