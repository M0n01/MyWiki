### Apache2
```bash
apt install apache2 '// Installation du serveur'
apt install iptables '// Installation du firewall'
iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT '// Autorisation du port tcp/80 en INPUT '
iptables -I INPUT -p tcp -m tcp --dport 443 -j ACCEPT '// Autorisation du port tcp/443 en INPUT' 
iptables-save | tee /etc/iptables/rules.v4 '// Sauvegarde de la config IPTABLES'
systemctl reload apache2 '// Reload du service'
a2enmod proxy proxy_fcgi '// Activation des modules'
systemctl restart apache2 '// Restart du service'
nano /etc/apache2/sites-available/wordpress.conf '// Création du VirtualHost pour Wordpress'
```
- Fichier de configuration Apache2 wordpress
```bash
<VirtualHost *:80>
ServerName wordpress.lan
DocumentRoot /var/www/wordpress/public
DirectoryIndex index.php
ProxyPassMatch "^/.*\.php$" "unix:/run/php/php7.4-wordpress.sock|fcgi://localhost/var/www/wordpress/public/"
ErrorLog ${APACHE_LOG_DIR}/wordpress-error.log
CustomLog ${APACHE_LOG_DIR}/wordpress-access.log combined
</VirtualHost>
```
- Activer, puis vérifier
```bash
a2ensite wordpress.conf '// Activer le VirtualHost'
systemctl restart apache2.service '// Relance du service'
apachectl configtest '// Test de configuration'
```
- Ajout d'un override DNS dans le fichier hosts sur l'hôte (C:\ Windows\ System32\ drivers\ etc\ hosts pour Windows, /etc/hosts pour Linux)
```bash
192.168.XX.XX wordpress.lan
```
