# DevSecOps-Pro
Notes for future / Exam

> Help commands
```bash
docker network --help
```
> to see available networks
```bash
docker network ls
```
> create new network
```bash
docker network create mynetwork
```
> explore more details of a network
```bash
docker inspect mynetwork
```

> attach network to a container
```bash
docker run -d --name ubuntu --network mynetwork -it ubuntu:18.04
```
> removing a network 
```bash
docker network rm mynetwork
```
> create macvlan network 
```bash
docker network create --driver macvlan mymacvlan
```

> this network disables networks inside container
```bash
docker run -d --name ubuntu --network=none -it ubuntu:18.04
```
> Create a network with subnet
```bash
docker network create app --subnet "172.10.2.0/16"
```
> connect app to a network
```bash
docker network connect app myubuntu
```
