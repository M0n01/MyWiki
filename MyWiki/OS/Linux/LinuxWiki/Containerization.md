#container #docker #lxc

#### Install Docker-Engine

Installing Docker is relatively straightforward. We can use the following script to install it on a Ubuntu host:

Code: bash

```bash
#!/bin/bash

# Preparation
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Add user htb-student to the Docker group
sudo usermod -aG docker htb-student
echo '[!] You need to log out and log back in for the group changes to take effect.'

# Test Docker installation
docker run hello-world
```

Le moteur Docker et des images Docker spécifiques sont nécessaires pour exécuter un conteneur.
La création d'une image Docker se fait en créant un [Dockerfile](https://docs.docker.com/engine/reference/builder/), qui contient toutes les instructions dont le moteur Docker a besoin pour créer le conteneur.

Nous pouvons utiliser les conteneurs Docker comme serveur « d'hébergement de fichiers » lors du transfert de fichiers spécifiques vers nos systèmes cibles. 

Par conséquent, nous devons créer un Dockerfile basé sur Ubuntu 22.04 avec le serveur Apache et SSH en cours d'exécution. Avec cela, nous pouvons utiliser **`scp`** pour transférer des fichiers vers l'image Docker, et Apache nous permet d'héberger des fichiers et d'utiliser des outils comme **`curl`**, **`wget`** et autres sur le système cible pour télécharger les fichiers requis. Un tel Dockerfile pourrait ressembler à ceci :
#### Dockerfile

Code: bash
the `FROM` statement, pecifies the base image. We can use it to customize an image for exemple.
```bash
# Use the latest Ubuntu 22.04 LTS as the base image
FROM ubuntu:22.04

# Update the package repository and install the required packages
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        openssh-server \
        && \
    rm -rf /var/lib/apt/lists/*

# Create a new user called "student"
RUN useradd -m docker-user && \
    echo "docker-user:password" | chpasswd

# Give the htb-student user full access to the Apache and SSH services
RUN chown -R docker-user:docker-user /var/www/html && \
    chown -R docker-user:docker-user /var/run/apache2 && \
    chown -R docker-user:docker-user /var/log/apache2 && \
    chown -R docker-user:docker-user /var/lock/apache2 && \
    usermod -aG sudo docker-user && \
    echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Expose the required ports
EXPOSE 22 80

# Start the SSH and Apache services
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```

Après avoir défini notre Dockerfile, nous devons le convertir en image. Avec la commande build, nous prenons le répertoire avec le Dockerfile, exécutons les étapes à partir du Dockerfile et stockons l'image dans notre Docker Engine local. Si l'une des étapes échoue en raison d'une erreur, la création du conteneur sera abandonnée. Avec l'option -t, nous attribuons une balise à notre conteneur, afin qu'il soit plus facile à identifier et à utiliser plus tard.

```bash
docker build -t FS_docker
```

Une fois l'image Docker créée, elle peut être exécutée via le moteur Docker

```bash
docker run -p <host port>:<docker port> -d <docker container name>
```
Exemple
```bash
docker run -p 8022:22 -p 8080:80 -d FS_docker
```

#### Cheat sheet

|                  |                               |
| ---------------- | ----------------------------- |
| `docker ps`      | List all running containers   |
| `docker stop`    | Stop a running container.     |
| `docker start`   | Start a stopped container.    |
| `docker restart` | Restart a running container.  |
| `docker rm`      | Remove a container.           |
| `docker rmi`     | Remove a Docker image.        |
| `docker logs`    | View the logs of a container. |




