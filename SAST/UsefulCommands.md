Use docker image that can be used for install and running, see master file

#### pip-audit
pip3 install pip-audit==1.1.2 
pip-audit -r ./requirements.txt -f json | tee pip-audit-output.json 

#### detect-secrets
pip3 install detect-secrets==1.3.0 
detect-secrets scan > secrets-output.json 
echo "q\n" | detect-secrets audit secrets-output.json 
 
#### Talisman
curl --silent https://raw.githubusercontent.com/thoughtworks/talisman/master/global_install_scripts/install.bash > /tmp/install_talisman.bash && /bin/bash /tmp/install_talisman.bash pre-commit 
curl --silent https://raw.githubusercontent.com/thoughtworks/talisman/master/global_install_scripts/install.bash > /tmp/install_talisman.bash && /bin/bash /tmp/install_talisman.bash pre-push 
source ~/.bashrc 
^ Adds it to the bash environment so you can call talisman -h 
talisman --scan 

#### SemGrep
pip3 install semgrep==0.108.0
semgrep --help\n
semgrep --lang python -e "os.system(...)" . --json | jq 
semgrep --lang python -e "DEBUG =True" --include settings.py . 
Scan all declarations of variables  
semgrep --lang python -e '$X = $Y' .
Scan all function calls that have request as an argument 
semgrep --lang python -e '$FUNC(request)' . 

#### Hadolint
wget https://github.com/hadolint/hadolint/releases/download/v1.18.0/hadolint-Linux-x86_64 
mv hadolint-Linux-x86_64 hadolint 
chmod +x hadolint 
./hadolint --help 
./hadolint Dockerfile 
cat -n Dockerfile This shows line numbers

#### FindSecBugs
wget https://github.com/WebGoat/WebGoat/releases/download/v8.1.0/webgoat-server-8.1.0.jar  
apt update && apt install openjdk-8-jre -y  
wget https://github.com/find-sec-bugs/find-sec-bugs/releases/download/version-1.9.0/findsecbugs-cli-1.9.0-fix2.zip && unzip findsecbugs-cli-1.9.0-fix2.zip -d /opt/  
sed -i -e 's/\r$//' /opt/findsecbugs.sh  
chmod +x /opt/findsecbugs.sh  
export PATH=/opt/:$PATH  
findsecbugs.sh -h  
findsecbugs.sh -progress -html -output findsecbugs-report.html webgoat-server-8.1.0.jar 

#### njsscan
pip3 install njsscan==0.3.3  
njsscan --help  
njsscan --json -o output.json .  

#### Pylint
pip3 install pylint
pylint --help  
pylint taskManager/*.py  
pylint taskManager/*.py -f json | tee output.json  
cat > .pylintrc <<EOF
[MASTER]
disable=missing-module-docstring,import-error
EOF  
pylint taskManager/*.py  

#### Brakeman
`apt update`  
`apt install ruby-full -y`  
`gem install brakeman -v 5.2.1`  
`brakeman -h`  
`brakeman -f json | tee result.json`  
create brakeman.ignore file if you want to ignore some tools

```
cat > brakeman.ignore <<EOF
{
    "ignored_warnings": [
        {
          "fingerprint": "febb21e45b226bb6bcdc23031091394a3ed80c76357f66b1f348844a7626f4df",
          "note": "ignore XSS"
        }
    ]
}
EOF```
`brakeman -f json -i brakeman.ignore | tee result.json`  


#### SonarQube
export SONAR_VERSION="4.7.0.2747"  
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_VERSION}-linux.zip -O /opt/sonar-scanner.zip  
unzip /opt/sonar-scanner.zip -d /opt/  
chmod +x /opt/sonar-scanner-${SONAR_VERSION}-linux/bin/sonar-scanner  
export PATH=/opt/sonar-scanner-${SONAR_VERSION}-linux/bin/:$PATH  
export SONARQUBE_TOKEN=INSERT_YOUR_TOKEN_HERE  
sonar-scanner -Dsonar.projectKey=Django -Dsonar.sources=. -Dsonar.host.url=https://sonarqube-xioqjd4c.lab.practical-devsecops.training -Dsonar.login=$SONARQUBE_TOKEN  





