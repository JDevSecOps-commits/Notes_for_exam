>Tools of the Trade
```bash

Basics


  CI/CD - Continous Integration/Continous Delivery:-
  
  Source Code Managment - Version Control Systems - Gitlab Ci/CD
  Artifact Management - maintain tightly auditable and deployable artifacts
  Docker Registry - store and retreive Docker images (deploy artifacts)
  IaC - Infrastructure as Code - Terraform, Ansible
  Monitoring - various checks such as database, cpu, RAM usage regular checks for ROI.
    
```

>Docker
```bash

Image - tar or zip file explaing how a container can be created from the image
Container - running image
Registry - place to store and retreive docker image
Repository - place to store a particular image and its version in a registry
```

>SCA
```bash
Software Component Analysis is a software technique to find security vulns in third-party projects
Static Analysis technique aka Software Component Analysis

**SCA Tools**

retirejs
  >Frontend
  retirejs - npm install -g retire # Install retirejs npm package
  retire --outputformat json --outputpath retirejs-report.json --severity high
  
  >Backend
  safety - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
  
>
```

>SAST Tools
```bash

>Trufflehog
secres-scanning - docker run --rm -v $(pwd):/src hysnsec/trufflehog file:///src --json | tee trufflehog-output.json
code analysis - docker run --user $(id -u):$(id -g) --rm -v $(pwd):/src hysnsec/bandit -r /src -f json -o /src/bandit-output.json
Safety - 
Bunlder Audit - 
Compsoer - 
OWASP Dependency Checker - 
```

>DAST
```bash
>SSL Scan
docker run --rm -v $(pwd):/tmp hysnsec/sslyze prod-xcs30z62.lab.practical-devsecops.training:443 --json_out /tmp/sslyze-output.json

>Nmap
docker run --rm -v $(pwd):/tmp hysnsec/nmap prod-xcs30z62 -oX /tmp/nmap-output.xml


>ZAP Baseline
docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-xcs30z62.lab.practical-devsecops.training -J zap-output.json

>OAST Tools - Component Analysis
```bash

```
>Notes
```bash
image: docker:latest

services:
  - docker:dind

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
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py check

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

# Software Component Analysis
sca-frontend:
  stage: build
  image: node:alpine3.10
  script:
    - npm install
    - npm install -g retire # Install retirejs npm package.
    - retire --outputformat json --outputpath retirejs-report.json --severity high
  artifacts:
    paths: [retirejs-report.json]
    when: always # What is this for?
    expire_in: one week
  allow_failure: true #<--- allow the build to fail but don't mark it as such  

sca-backend:
  stage: build
  script:
    - docker pull hysnsec/safety
    - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
  artifacts:
    paths: [oast-results.json]
    when: always # What does this do?
  allow_failure: true #<--- allow the build to fail but don't mark it as such

# Git Secrets Scanning
secrets-scanning:
  stage: build
  script:
    - docker run -v $(pwd):/src --rm hysnsec/trufflehog --repo_path /src file:///src --json | tee trufflehog-output.json
  artifacts:
    paths: [trufflehog-output.json]
    when: always  # What is this for?
    expire_in: one week
  allow_failure: true

# Static Application Security Testing
sast:
  stage: build
  script:
    - docker pull hysnsec/bandit  # Download bandit docker container
    # Run docker container, please refer docker security course, if this doesn't make sense to you.
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
  allow_failure: true   #<--- allow the build to fail but don't mark it as such

# Dynamic Application Security Testing
nikto:
  stage: integration
  script:
    - docker pull hysnsec/nikto
    - docker run --rm -v $(pwd):/tmp hysnsec/nikto -h prod-xcs30z62 -o /tmp/nikto-output.xml
  artifacts:
    paths: [nikto-output.xml]
    when: always

sslscan:
  stage: integration
  script:
    - docker pull hysnsec/sslyze
    - docker run --rm -v $(pwd):/tmp hysnsec/sslyze prod-xcs30z62.lab.practical-devsecops.training:443 --json_out /tmp/sslyze-output.json
  artifacts:
    paths: [sslyze-output.json]
    when: always

nmap:
  stage: integration
  script:
    - docker pull hysnsec/nmap
    - docker run --rm -v $(pwd):/tmp hysnsec/nmap prod-xcs30z62 -oX /tmp/nmap-output.xml
  artifacts:
    paths: [nmap-output.xml]
    when: always

zap-baseline:
  stage: integration
  script:
    - docker pull owasp/zap2docker-stable:2.10.0
    - docker run --user $(id -u):$(id -g) --rm -v $(pwd):/zap/wrk:rw owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-xcs30z62.lab.practical-devsecops.training -J zap-output.json
  after_script:
    - docker rmi owasp/zap2docker-stable:2.10.0  # clean up the image to save the disk space
  artifacts:
    paths: [zap-output.json]
    when: always # What does this do?
  allow_failure: false
  ```

>Safety
>```bash
>Safety checks Python dependencies for known security vulnerabilities and suggests the proper remediations for vulnerabilities detected.
```


>Nmap
```bash
Nmap scans ports and network
```


>Ansible
```bash
>Ansible manages servers
```

>SSLyze
```bash
>SSLyze checks SSL security
```

>Software Component Analysis
```bash
Software Component Analysis is a technique used to find vulns in third-party components
```
>OAST TOOLS
```bash
retirejs
Safety
Bulnder Audit
Composer
```

>Info
```bash
For python, the package manager is pip and dependency file is requirements.txt

For node.js, the package manager is npm and dependency file is package.json

For ruby, the package manager is rubygems and dependency file is gemfile

For java, the package manager is maven and dependency file is pom.xml

The list goes on, a google search will usually tell you where dependencies are stored.



Note: Some SCA tools like Snyk, need the respective package manager to be installed before they can scan the dependencies for a package.

e.g, if a project has both package.json (Javascript/node.js) and requirements.txt(python) then snyk expects npm and pip to be installed on that machine otherwise it won't scan dependencies.



If npm is installed, it will process package.json and will ignore requirements.txt

If pip is installed on the machine, it will process requirements.txt. It won't process package.json or error out.
```

>STA Tools
```bash
Bandit
Brakeman
Trufflehog

```
>Trufflehog
```bash
TruflleHog is a tool that searches through git repositories for secrets, digging deep into commit history and branches. This tool is useful in finding the secrets accidentally committed to the repo.
```

>Stuff
```bash
git-secrets:
  stage: build
  script:
    - docker run -v $(pwd):/src --rm hysnsec/trufflehog --repo_path /src file:///src --json | tee trufflehog-output.json
  artifacts:
    paths: [trufflehog-output.json]
    when: always  # What is this for?
    expire_in: one week
```

>DAST Tools
```bash
ZAP
Burp Suite
Nikto
Nmap
```

>Info2
```bash
inspec:
  stage: prod
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -t rsa $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - docker run --rm -v ~/.ssh:/root/.ssh -v $(pwd):/share hysnsec/inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@$DEPLOYMENT_SERVER -i ~/.ssh/id_rsa --chef-license accept
```


>Bandit
```bash
The Bandit is a tool designed to find common security issues in Python code.

To do this Bandit, processes each file, builds an AST, and runs appropriate plugins against the AST nodes. Once Bandit has finished scanning all the files it generates a report.
```
