Expects a docker-compose.yml file 

example docker-compose file:
```
cat >docker-compose.yml<<EOF
version: "3"

services:
  ubuntu:
    image: ubuntu:18.04
    container_name: ubuntu1
    stdin_open: true        # the same way like docker run -i
EOF
```

`docker-compose up -d`  
Naturally checks for docker-compose.yml in the working dir
Remove all containers  
`docker rm -f $(docker ps -aq)`  
`docker-compose down`  

Running multiple containers yaml file example:  
```
version: "3"

services:
  ubuntu:
    image: ubuntu:18.04
    container_name: myubuntu
    volumes:
      - data:/opt

  alpine:
    image: alpine:3.13
    container_name: myalpine
    volumes:
      - data:/tmp

volumes:
  data:
```
Check the container processes
`docker-compose ps`

Another example
```
cat > /opt/docker-compose.yml<<EOF
version: "3"
services:
  webserver:
    image: nginx
    container_name: webserver
    ports:
     - 8080:80
    volumes:
     - data:/usr/share/nginx/html

volumes:
  data:
EOF
```
`docker-compose -f /opt/docker-compose.yml up -d`  
`docker exec webserver sh -c 'echo "hello world" > /usr/share/nginx/html/hello.html'`
`docker-compose exec webserver sh -c 'echo "hello world" > /usr/share/nginx/html/hello.html'`
last two are equivalent

Instructions:
- version:        Version of compose file format to use
- image:          Specify the image to run
- container_name: Specify a custom container name, rather than a default name
- ports:          Expose port(s), similar to docker run -p argument
- environment:    Add environemtn variables into the contianer by defining a key-value pair
- volumes:        Volumes to save our data persistently using various options type like bind or volumes

