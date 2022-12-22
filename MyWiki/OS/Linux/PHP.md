### PHP
```bash
apt install php7.4-fpm php7.4-mysql php7.4-xml php7.4-mbstring '// Installation des paquets PHP + FPM'
systemctl status php7.4-fpm.service '// Check du service PHP'
sudo mkdir -p /var/www/wordpress/public '// Création du dossier Wordpress/Public/'
sudo useradd -s /usr/sbin/nologin -d /var/www/wordpress -M wpuser '// Création de wpuser sans MDP pour le dossier /var/www/wordpress'
sudo chown -R wpuser:wpuser /var/www/wordpress '// Ajout des droits a wpuser sur /var/www/wordpress'
rm /etc/php/7.4/fpm/pool.d/www.conf '// Suppression du pool FPM par défaut'
nano /etc/php/7.4/fpm/pool.d/wordpress.conf '// Création du pool pour Wordpress'
```
- Fichier de configuration wordpress
```bash
[wordpress] 
chdir = /var/www/wordpress/public 
user = wpuser 
group = wpuser 
listen = /run/php/php7.4-$pool.sock 
listen.mode = 0660 
listen.owner = www-data 
listen.group = www-data 
pm = ondemand 
pm.max_children = 2 
```
- Rédemarrer le service PHP
```bash
systemctl restart php7.4-fpm.service '// Redémarrage de PHP pour application des changements'
```
