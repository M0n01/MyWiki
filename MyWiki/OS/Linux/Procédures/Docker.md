
## ==Mission 1==


```bash
sudo service docker start  // ou status

sudo systemctl disable docker // désactiver lancement docker au démarrage
```

### Complete the file 

nano Dockerfile
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
```bash
docker build -t my_docker_build LP/A2SR/Course_1/app/

docker run -p 127.0.0.1:3000:3000 my_docker_build
```

Push une image Docker dans un "Repositorie"
```bash
docker login --username=xitix

docker tag todolist_a2sr_v2 xitix/todolist_a2sr_v2

docker image push xitix/todolist_a2sr_v2
```

Pour télécharger une image Docker 
```bash
docker image pull xitix/todolist_a2sr_v2
```

Pour voir la liste des images
```bash
docker image ls
```  

Pour supprimer tous les containers et images 
```bash
docker system prune -a
```
