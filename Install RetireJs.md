# DevSecOps-Pro
Notes for future / Exam
> install NPM
```bash
curl -sL https://deb.nodesource.com/setup_12.x | bash -
```
> install nodejs
```bash
apt install nodejs -y
```
>install retirejs
```bash
npm install -g retire@3.0.6
```

>Run scan
```bash
retire --outputformat json --outputpath retire_output.json
```
>Embed retirejs
```bash
oast-frontend:
 stage: test
 image: node:alpine3.10
 script:
   - npm install
   - npm install -g retire
   - retire --outputformat json --outputpath retirejs-report.json --severity high
 artifacts:
   paths: [retirejs-report.json]
   when: always
   expire_in: one week # Optional
```
