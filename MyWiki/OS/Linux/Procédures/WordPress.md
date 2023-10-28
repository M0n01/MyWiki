
### WordPress
- Récupérer le dossier Wordpress sur le partage
- Se connecter avec l'utilisateur wpftp sur WinSCP/Filezilla
- Déplacer le dossier Wordpress dans le dossier Public/
```bash
apt install zip '// Installation du paquet'
cd /var/www/wordpress/public '// Changement de répertoire'
unzip wordpress-6.1.1-fr_FR.zip  '// Décompression du fichier'
rm wordpress-6.1.1-fr_FR.zip
cp -rf wordpress/* /var/www/wordpress/public '// Déplacement de tous les fichiers'
rm -rf wordpress/
```
- Finir l'installation sur l'interface WEB de Wordpress
```bash
http://wordpress.lan
```
- Indiquer la BDD, Utilisateur SQL

![image](https://i.imgur.com/9D0RjRm.png)

- Cliquer sur Run the installation

![image](https://user-images.githubusercontent.com/73076854/206922743-8f158e68-325e-4cf5-8d64-4e57e8810708.png)

- Définir le titre, user, password, email

![image](https://user-images.githubusercontent.com/73076854/206922785-f2bd8c90-47f9-47bf-88f2-dda17261cde6.png)

- Cliquer sur Log In

![image](https://user-images.githubusercontent.com/73076854/206922805-9ffc6c7d-467d-450e-92ab-77343ff8c9fe.png)

- Remplir le champ de connexion 

![image](https://user-images.githubusercontent.com/73076854/206922818-1d8d5747-02e0-4e1b-a200-7086eaeacbcd.png)

- Et Voila !
![image](https://user-images.githubusercontent.com/73076854/206922835-a67b5c22-ba82-4033-967f-59b8157ec02a.png)

