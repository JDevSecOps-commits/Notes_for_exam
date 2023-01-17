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
>SCA (OAST)
```bash
Software Component Analysis is a software technique to find security vulns in third-party projects
Static Analysis technique aka Software Component Analysis

Strengths - Less false positives than SAST
Weaknesses - Uses checksums of files and packages to find vulns. Not fit for internal or non-analysed component by the vendor
Threats - False positives and license checks can be vague.
Opportunities - Ability to scan code to see if can be improved

Used on third-party components, internal developed components and docker containers
**SCA Tools**
Retirejs
>
Detects known vulns in Javascript libraries
>
  >Frontend
  retirejs - npm install -g retire # Install retirejs npm package
  retire --outputformat json --outputpath retirejs-report.json --severity high
  
  >Backend
  safety - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
 >
 
Safety
  >
  Scans known vulns issues in python packages
  safety - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
  >
```
>SAST Info
```bash
Scans source code, binary and byte code without running the code
>SAST Tools
```bash
>Bandit
?
>
>Brakeman
?
>
>Trufflehog
Secrets Scanning
>
```

>DAST
```bash
>SSL Scan
docker run --rm -v $(pwd):/tmp hysnsec/sslyze prod-xcs30z62.lab.practical-devsecops.training:443 --json_out /tmp/sslyze-output.json

>Nmap
docker run --rm -v $(pwd):/tmp hysnsec/nmap prod-xcs30z62 -oX /tmp/nmap-output.xml

>Nikto


>ZAP Baseline
docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-xcs30z62.lab.practical-devsecops.training -J zap-output.json


