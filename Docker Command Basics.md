# DevSecOps-Pro
Notes for future / Exam
> Pull image from DockerHub
```bash
docker pull ubuntu
```
> Hide output using -q 
```bash
docker pull -q alpine
```

> Start container from docker image
```bash
docker run -d --name myubuntu ubuntu
```
> List docker container
```bash
docker ps
```

> to keep docker container running
```bash
docker run -d --name myubuntu -i ubuntu
```

> remove docker container 
```bash
docker rm myubuntu
```
> run command inside a container using exec
```bash
docker exec myubuntu whoami
docker exec -it myubuntu bash
docker exec myubuntu cat /etc/lsb-release
```
>stop and start docker container
```bash
docker stop webserver && docker rm webserver
```
> Check Docker Status
```bash
docker ps -a
```
> Show processes running inside a container 
```bash
docker top myubuntu
```
> Download Source Code
```bash 
git clone https://gitlab.practical-devsecops.training/pdso/django.nv.git
```
> Build Docker Image
```bash
docker build -t django.nv:1.0 .
```
> See Docker Imager
```bash
docker images
```
> docker rename images
```bash
docker tag django.nv:1.0 django.nv:1.1
```
> Manage Volune of docker container
```bash
docker volume --help
```
> List available volumes on your system
```bash
docker volume ls
```
> Create a new volume
```bash
docker volume create data
```
> run a contaner with file
```bash
docker run --name volumemount -v data:/src ubuntu:18.04 "/bin/bash" "-c" "echo test>>/src/hello.txt"
```

> Login to docker when using Gitlab
```bash
release:
  stage: release
  before_script:
   - echo $CI_REGISTRY_PASS | docker login -u $CI_REGISTRY_USER --password-stdin $CI_REGISTRY
  script:
   - docker build -t $CI_REGISTRY/root/django-nv .   # Build the application into Docker image
   - docker push $CI_REGISTRY/root/django-nv         # Push the image into registry
```

> SSh into prod machine and set up Docker
```bash
prod:
  stage: prod
  image: kroniak/ssh-client:3.6
  environment: production
  only:
      - master
  before_script:
   - mkdir -p ~/.ssh
   - echo "$PROD_SSH_PRIVKEY" > ~/.ssh/id_rsa
   - chmod 600 ~/.ssh/id_rsa
   - eval "$(ssh-agent -s)"
   - ssh-add ~/.ssh/id_rsa
   - ssh-keyscan -t rsa $PROD_HOST >> ~/.ssh/known_hosts
  script:
   - echo
   - |
      ssh root@$PROD_HOST << EOF
        docker login -u ${CI_REGISTRY_USER} -p ${CI_REGISTRY_PASS} ${CI_REGISTRY}
        docker rm -f django.nv
        docker pull ${CI_REGISTRY}/root/django-nv
        docker run -d --name django.nv -p 8000:8000 ${CI_REGISTRY}/root/django-nv
      EOF
  ```
