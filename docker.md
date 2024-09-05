# Docker setup

## installation 

### enable kvm

```
sudo modprobe kvm
```

### check kvm 

```
lsmod | grep kvm
```

### install docker

follow this guide (use apt repository)
https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

### Docker workshop

```
https://docs.docker.com/get-started/workshop/
```

## useful commands

### check docker status

```
systemctl status docker
```

### Create Dockerfile

create Dockerfile (without file extension) in project root directory

example file content

```
# syntax=docker/dockerfile:1

FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

### create image using Dockerfile

cd into project root

```
sudo docker build -t getting-started .
```

### run created image

```
sudo docker run -dp 127.0.0.1:3000:3000 getting-started
```
this creates a container, dispatched, port 3000 mapped to 127.0.0.1:3000 on host machine

### check running containers

```
sudo docker ps
```

### check available images

```
sudo docker images
```

### change image tag

```
sudo docker tag [old-tag] [new-tag]
```

### login to docker hub

```
sudo docker login -u chamikaratdp
```

### push image to docker hub

```
sudo docker push [image-tag] 

```

### create volume 

```
sudo docker volume create todo-db
```

### list available volumes

```
sudo docker volume list
```

### check volume details

```
sudo docker volume inspect [volume-name]
```

### run container with volume mounted

```
sudo docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
```

### use bind mounts

```
sudo docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash

sudo docker run -dp 127.0.0.1:3000:3000 -w /app --mount type=bind,src="$(pwd)",target=/app node:18-alpine sh -c "yarn install && yarn run dev"
```


