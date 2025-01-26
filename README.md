# Docker Swarm via multipass

## Setup

```bash

multipass networks

multipass launch --name swarm-manager --mem 2G --disk 10G --cpus 2 --network en0
multipass launch --name swarm-worker --mem 2G --disk 10G --cpus 2 --network en0

multipass list

multipass shell swarm-manager
multipass shell swarm-worker


## on both nodes
sudo snap install docker
## so we can use docker without sudo (on both nodes)
sudo groupadd docker
sudo usermod -aG docker ubuntu
newgrp docker
sudo chown root:docker /var/run/docker.sock


# on manager
docker swarm init --advertise-addr $(hostname -I | awk '{print $1}')

# on worker
docker swarm join --token SWMTKN-1-1qviivdzd0rhm3nlwd1mul52lip6ic809lathsj14e5bdm3vlq-abs235dcy4h3dv7e9bssqwhco 192.168.64.19:2377 

# manager

docker node ls

docker service create --name nginx \
  --publish 8080:80 \
  --replicas 2 \
  nginx

docker service ls

docker service ps nginx

curl http://192.168.64.21:8080
```

## Using stack

```bash
docker stack deploy -c stack.yml app
```