### FTP
```bash
apt install vsftpd libpam-pwdfile '// Installation du serveur FTP'
iptables -I INPUT -p tcp -m tcp --dport 21 -j ACCEPT '// Autorisation du port tcp/80 en INPUT'
iptables -A INPUT -p tcp --match multiport --dports 20000:20200 -j ACCEPT '// Autorisation des ports tcp/20000 à 20200 en INPUT'
iptables-save | tee /etc/iptables/rules.v4 '// Sauvegarde de la config IPTABLES'
mkdir -p /etc/vsftpd/users.d '// Création dossier de config'
nano /etc/pam.d/vsftpd '// Modifier la configuration PAM pour vsFTPd'
```
- Supprimer toutes les lignes du fichier, puis les remplacer
```bash
auth required pam_pwdfile.so pwdfile /etc/vsftpd/vsftpd.passwd
account required pam_permit.so
session required pam_permit.so
```

```bash
nano /etc/vsftpd.conf '// Fichier de conf'
```
- Ajouter les lignes suivantes en bas du fichier
```bash
pasv_min_port=20000
pasv_max_port=20200
chroot_local_user=YES
allow_writeable_chroot=YES
write_enable=YES
virtual_use_local_privs=YES
guest_enable=YES
hide_ids=YES
user_config_dir=/etc/vsftpd/users.d
```
- Redémarrer le servive vsFTPd
```bash
systemctl restart vsftpd.service '// Redémarrer le démon'
```
- Ajouter les lignes suivantes en bas du fichier
```bash
nano /etc/vsftpd/users.d/wpftp

guest_username=wpuser
local_root=/var/www/wordpress
```
- Créer un mot de passe pour le compte FTP
```bash
echo "wpftp:$(openssl passwd -1)" | tee -a /etc/vsftpd/vsftpd.passwd
```