
## ==Mission 1==

apt update

modprobe kvm

modprobe kvm_amd

usermod -aG kvm $USER
  
``` bash
sudo apt-get update
sudo apt-get install ca-certificates
sudo apt-get install curl
sudo apt-get install gnupg
sudo apt-get install lsb-release

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo docker run hello-world

sudo apt-get update
https://docs.docker.com/desktop/install/debian/
sudo apt-get install ./docker-desktop-<version>-<arch>.deb
systemctl --user start docker-desktop
systemctl --user enable docker-desktop
```

### GIT

```bash
apt install git

git clone https://github.com/mperochon/A2SR

cd A2SR/Course_1/app

nano Dockerfile
```

### Complete the file 

```
# Complete the file :)

# Description : It is the image use as base

FROM node:18-alpine

ENV HTTP_PROXY 'http://192.168.2.7:3128'
ENV HTTPS_PROXY 'http://192.168.2.7:3128'

RUN npm config set proxy http://192.168.2.7:3128
RUN npm config set https-proxy http://192.168.2.7:3128

# Explain : RUN is used to execute command in our contener

#RUN apk add --no-cache python2 g++ make

# Explain : It is used to modify the repository (it's like cd).
# So all the following command will be executed in this repo

WORKDIR /app

# Description : To bundle the app's source code inside the Docker image

COPY . .

RUN npm install --production

# Explain : CMD is always at the end.
# It allow the container to know wich command execute at the beginning
# It is for run the app. Here we will use index.js to start

CMD ["node", "src/index.js"]
```

### Build and run the container
```
root@debian11:~# docker build -t my_docker_build LP/A2SR/Course_1/app/

root@debian11:~# docker run -p 127.0.0.1:3000:3000 my_docker_build
```


## ==Mission 2==

Modifier app.js (remplacer un mot pour le rendre unique)
```
root@debian11:~# nano LP/A2SR/Course_1/app/src/static/js/app.js
```

Créer via le site un "Repositorie"
```
root@debian11:~# docker login --username=xitix

root@debian11:~# docker tag todolist_a2sr_v2 xitix/todolist_a2sr_v2

root@debian11:~# docker image push xitix/todolist_a2sr_v2
```

Pour télécharger une image Docker 
```
root@debian11:~# docker image pull xitix/todolist_a2sr_v2
```

Pour voir la liste des images
```
root@debian11:~# docker image ls
```  

Pour supprimer tous les containers et images 
```
root@debian11:~# docker system prune -a
```
