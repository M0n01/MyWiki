
### MariaDB
```bash
apt install mariadb-server '// Installation du serveur SQL'
mysql -u root -p '// Connexion console MySQL'
```
- Créer la BDD wpdb, ainsi qu'un utilisateur
```bash
CREATE DATABASE wpdb;
CREATE USER 'wordpressadmin'@'localhost' IDENTIFIED BY 'Toptop=22';
GRANT ALL PRIVILEGES ON wpdb.* TO wordpressadmin@localhost;
FLUSH PRIVILEGES;
exit
```
