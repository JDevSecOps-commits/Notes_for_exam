# DevSecOps-Pro
Notes for future / Exam

> Pull Zaps docker Image
```bash
docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py --help
```

> Help for Zaps
```bash
docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py --help
```
> Run Zap with basic usage
```bash
docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-eggkd08x.lab.practical-devsecops.training
```
> Run Zap but with json output
```bash
docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-eggkd08x.lab.practical-devsecops.training -J zap-output.json
```

>Embed Zap
```bash
image: docker:latest  # To run all jobs in this pipeline, use a latest docker image

services:
  - docker:dind       # To run all jobs in this pipeline, use a docker image which contains a docker daemon running inside (dind - docker in docker). Reference: https://forum.gitlab.com/t/why-services-docker-dind-is-needed-while-already-having-image-docker/43534

stages:
  - build
  - test
  - release
  - preprod
  - integration
  - prod

build:
  stage: build
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env                       # Create a virtual environment for the python application
   - source env/bin/activate              # Activate the virtual environment
   - pip install -r requirements.txt      # Install the required third party packages as defined in requirements.txt
   - python manage.py check               # Run checks to ensure the application is working fine

test:
  stage: test
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py test taskManager

zap-baseline:
  stage: integration
  script:
    - docker pull owasp/zap2docker-stable:2.10.0
    - docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-z3l8hs55.lab.practical-devsecops.training -J zap-output.json
  after_script:
    - docker rmi owasp/zap2docker-stable:2.10.0  # clean up the image to save the disk space
  artifacts:
    paths: [zap-output.json]
    when: always # What does this do?
  allow_failure: false
  ```
