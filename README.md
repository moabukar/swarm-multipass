# Docker Swarm via multipass

## Setup

```bash
multipass launch --name swarm-manager --mem 2G --disk 10G --cpus 2
multipass launch --name swarm-worker --mem 2G --disk 10G --cpus 2

## on both nodes
sudo snap install docker
## so we can use docker without sudo (on both nodes)
sudo groupadd docker
sudo usermod -aG docker ubuntu
newgrp docker
sudo chown root:docker /var/run/docker.sock
```