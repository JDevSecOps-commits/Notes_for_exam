# DevSecOps-Pro
Notes for future / Exam
> Dockerfile
```bash
cat > Dockerfile <<EOL
FROM ubuntu:18.04

RUN apt update && apt install nginx -y

CMD ["/bin/bash", "-c" , "service nginx start"]
CMD [";sleep infinity"]
EOL
```
