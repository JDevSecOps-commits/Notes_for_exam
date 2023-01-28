Example Regular run with regular mount
docker run -v $PWD:/CHOSEN_DIR -w /CHOSEN_DIR hello-world:latest bin/sh
#### Example Dockerfile
```
# FROM python base image
FROM python:2-alpine

# COPY startup script
COPY . /app

WORKDIR /app

RUN apk add --no-cache gawk sed bash grep bc coreutils
RUN pip install -r requirements.txt
RUN chmod +x reset_db.sh run_app_docker.sh && bash reset_db.sh

# EXPOSE port 8000 for communication to/from server
EXPOSE 8000

# CMD specifcies the command to execute container starts running.
CMD ["/app/run_app_docker.sh"]
```

#### Basic Commands
-d option runs a container in the background 
-name gives it a name, (to see when running docker ps)
-i = interactive, adds a process to keep it going after run
`docker exec` to run command in existing container  
`docker run -d --name myubuntu -i ubuntu`  
`docker exec myubuntu cat /etc/lsb-release`    
`docker exec -it myubuntu bash`  
`docker ps -a` (-a option shows terminated containers)  
`docker top ubuntu`  Displays the running processes
`docker run -d --name webserver -e APP=nginx -p 80:80 nginx:1.21.3`  g
-p binds container port 80 to host port 80
#### Images
`docker build -t TAGNAME:TAGVER .` Directory with Dockerfile
`docker images`   
`docker tag django.nv:1.0 django.nv:1.1` 
`docker run --name webserver -it --entrypoint /bin/bash django.nv:1.0`  
`docker rmi django.nv:1.0`  
#### Data
List available volumes: `docker volume ls`
`docker volume create demo ` 
`ls /var/lib/docker/volumes/` Volumes dir 
`docker run --name ubuntu -d -v demo:/opt -it ubuntu:18.04` maps existing volume
`ls /var/lib/docker/volumes/demo/_data/` data persists here

with tmpfs not persistent, data stored in RAM
`docker run --tmpfs hello-world:latest /bin/sh`

Mounting a file 
`docker run --name bindmount -v /opt/hello.txt:/src/hello.txt ubuntu:18.04`

#### Docker Registry
Create a registry
`docker run -d -p 5000:5000 --restart=always --name registry registry:2`
`docker tag django.nv:1.0 localhost:5000/django.nv:1.0`  
`docker push localhost:5000/django.nv:1.0`  
`curl localhost:5000/v2/_catalog` to see the image
`docker tag django.nv:1.0 dockerhubusername/django.nv:1.0`  
`docker push dockerhubusername/django.nv:1.0`  
`docker stop registry` 
`docker rmi django.nv:1.0` 

#### Docker Networking 
Info on different network types at bottom.
`docker network ls` available networks
`docker network create mynetwork`  
`docker inspect mynetwork`  
`docker run -d --name ubuntu --network mynetwork -it ubuntu:18.04`  Attach network to container
`docker rm -f ubuntu`  
`docker network rm mynetwork`  
`docker network create --driver macvlan mymacvlan`  
'ifconfig' will show the mac 
`docker inspect ubuntu -f "{{json .NetworkSettings.Networks }}" | jq` check network info on container
`docker run -d --name ubuntu --network=none -it ubuntu:18.04`  
`docker network create app --subnet "172.10.2.0/16`  
`docker network connect app myubuntu` attach network to container

#### Dockerfile
```
cat > Dockerfile <<EOL
FROM ubuntu:18.04

RUN apt update && apt install nginx -y

CMD ["/bin/bash", "-c" , "service nginx start; sleep infinity"]
EOL
```
```
cat > Dockerfile <<EOL
FROM ubuntu:18.04

RUN apt update && apt install nginx -y

ENTRYPOINT ["/bin/bash", "-c"]

CMD ["service nginx start; sleep infinity"]
EOL
```
Entrypoint always runs first but can be overridden.

#### noexec etc
`docker run -d --tmpfs /run:rw,noexec,nosuid,size=65536k my_image`
volume mounting options
rw - read write mode  
noexec - No execution 
nosuid - Cannot contain set uid files. i.e files that allow file to be executed with the permissions of another user  

#### Create a snapshot in docker
First run the container 
`docker run -d --name ubuntu -i ubuntu:18.04`  
`docker exec -it ubuntu bash`  
`apt update && apt install -y nginx`  
`docker save ubuntu > ubuntu-save.tar`  
`docker export ubuntu > ubuntu-export.tar`  
save only saves image layers, history and deleted/overridden files. export also saves the current state of the container even after we executed the command like apt update and also installed the package 

Import snapshot and run it 
`docker load -i ubuntu-save.tar`
`docker import ubuntu-export.tar ubuntu-export` 
DOESN'T SAVE THE VOLUME, NEED TO SAVE SEPERATELY

#### Network types
##### bridge
This is the default network driver when you don’t specify a driver for the containers. Containers on the same bridged network can speak to each other, but are isolated from containers on other bridged networks. All containers can access the external network through NAT
##### host	
The containers use the host’s networking directly, while retaining separation on storage and processing. Ports exposed by the container are exposed on the external network using the host’s IP address
##### macvlan	
When creating a macvlan, you assign a parent network device (e.g. “eth0”). Each container on the macvlan network will receive its own MAC address on the network that eth0 is connected to. Each container has full network access. Warning: when misconfigured, you may overrun the network with too many MACs, or you may duplicate IP addresses
##### none	
networking is disabled with this network driver, containers cannot communicate to each other, nor with the external network


