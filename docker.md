# Docker setup



## Things to remember

- Remember that each container has its own IP address.
- the container only starts one process
- it's best to run your app in multiple containers.
- While using env vars to set connection settings is generally accepted for development, it's highly discouraged when running applications in production.
- Once a layer changes, all downstream layers have to be recreated as well.
- A Dockerfile provides instructions to build a container image while a Compose file defines your running containers.




## Further reading

- https://diogomonica.com/2017/03/27/why-you-shouldnt-use-env-variables-for-secret-data/
- https://docs.docker.com/build/building/best-practices/
- https://docs.docker.com/get-started/workshop/10_what_next/

- https://landscape.cncf.io/


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

### stop container

```
sudo docker stop <container_id>
```

### remove container

stopped one

```
sudo docker rm <container_id>
```

without stopping

```
sudo docker rm -f <container_id>
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

### check container logs

```
docker logs -f <container-id>
```

### execute command in running container

```
docker exec -it <mysql-container-id> command
```

### see the command that was used to create each layer within an image.

```
docker image history --no-trunc getting-started
```













## Multi container apps

If you place the two containers on the same network, they can talk to each other

### create network

```
sudo docker network create todo-app
```

### run mysql container withing the network

note the --network-alias flag, this can be used to identify the container

```
docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:8.0
```

### verify mysql db 

```
docker exec -it <mysql-container-id> mysql -u root -p
```

```
mysql> SHOW DATABASES;
```

```
mysql> exit
```


### Use netshoot to debugging networking issues

run netshoot container

```
docker run -it --network todo-app nicolaka/netshoot
```

```
dig mysql
```

should be able to see an ***A*** record with hostname ***mysql*** with it's ip address


### Start app with mysql connection

cd into app directory

```
docker run -dp 127.0.0.1:3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:18-alpine \
  sh -c "yarn install && yarn run dev"
```















## Compose

- The name will automatically become a network alias
- Docker Compose creates a network specifically for the application stack

example ***compose.yaml*** file

```
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

### run application stack

cd into app directory

```
docker compose up -d
```

### to see logs

all services

```
docker compose logs -f
```

one service  (eg:- app) 

```
docker compose logs -f app
```

### stop application

By default, named volumes in your compose file are not removed, add ***--volumes*** flag if needed

```
docker compose down
```











## Multi-stage builds

- https://docs.docker.com/get-started/workshop/09_image_best/ 


```
# syntax=docker/dockerfile:1
FROM node:18 AS build
WORKDIR /app
COPY package* yarn.lock ./
RUN yarn install
COPY public ./public
COPY src ./src
RUN yarn run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```