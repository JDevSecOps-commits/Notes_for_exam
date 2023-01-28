##### Safety
`pip3 install safety==2.3.5`
##### pip-audit
`pip3 install pip-audit==1.1.2`
##### RetireJS
```
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt install nodejs -y
npm install -g retire@3.0.6
```
##### Dependency-check
```
apt update
apt install openjdk-8-jre -y
wget -O /opt/v6.1.6.zip https://github.com/jeremylong/DependencyCheck/releases/download/v6.1.6/dependency-check-6.1.6-release.zip
unzip /opt/v6.1.6.zip -d /opt/
export PATH=/opt/dependency-check/bin:$PATH
dependency-check.sh -h
dependency-check.sh --scan /webapp --format "CSV" --project "Webgoat" --failOnCVSS 8 --out /opt
```
##### Snyk
```
wget -O /usr/local/bin/snyk https://github.com/snyk/cli/releases/download/v1.984.0/snyk-linux
chmod +x /usr/local/bin/snyk
snyk auth API_TOKEN_HERE
curl -sL https://deb.nodesource.com/setup_12.x | bash - # Install npm
apt install nodejs -y
npm install # Cannot run without installing depencencies
```
##### npm-audit
```
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt install nodejs -y
npm audit -h
npm audit --json | tee results.json
```
##### AuditJS
```
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt install nodejs -y
npm install -g auditjs
npm install # In the project
auditjs ossi -q -j | tee auditjs-output.json
```
##### Bundler-Audit
```
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
export PATH="~/.rbenv/bin:$PATH"
cat Gemfile # CHECK the ruby project to see what needs to be involved 
apt update
apt-get install build-essential libreadline-dev -y
rbenv install --verbose 2.6.5
export PATH="/root/.rbenv/versions/2.6.5/bin:$PATH"
gem install --user-install bundler-audit
export PATH="~/.gem/ruby/2.6.0/bin/:$PATH"
```
##### Chelsea
```
curl -sSL https://rvm.io/mpapis.asc | gpg2 --import - curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import - curl -sSL https://get.rvm.io | bash -s stable
source /etc/profile.d/rvm.sh
cat Gemfile # See what version of ruby needs installing
rvm install "ruby-2.6.5"
gem install chelsea -v 0.0.27
chelsea -f Gemfile.lock -t json
```

##### Trufflehog
```pip3 install trufflehog==2.2.1```
##### detect-secrets
```pip3 install detect-secrets==1.3.0```
##### Talisman
```curl --silent https://raw.githubusercontent.com/thoughtworks/talisman/master/global_install_scripts/install.bash > /tmp/install_talisman.bash && /bin/bash /tmp/install_talisman.bash pre-commit
curl --silent https://raw.githubusercontent.com/thoughtworks/talisman/master/global_install_scripts/install.bash > /tmp/install_talisman.bash && /bin/bash /tmp/install_talisman.bash pre-push
```
##### Bandit
`pip install bandit==1.7.4`
##### GoSec
```
curl -s https://dl.google.com/go/go1.17.4.linux-amd64.tar.gz | tar xvz -C /usr/local
export GOROOT=/usr/local/go 
export GOPATH=$HOME/go 
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
curl -sfL https://raw.githubusercontent.com/securego/gosec/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v2.4.0
go get -u github.com/securego/gosec/v2/cmd/gosec
gosec ./...
gosec -exclude=G104 ./...
```

##### Semgrep
`pip3 install semgrep==0.108.0`
##### Hadolint
```
wget https://github.com/hadolint/hadolint/releases/download/v1.18.0/hadolint-Linux-x86_64
mv hadolint-Linux-x86_64 hadolint
chmod +x hadolint
./hadolint Dockerfile
```
##### FindSecBugs
```
wget https://github.com/WebGoat/WebGoat/releases/download/v8.1.0/webgoat-server-8.1.0.jar
apt update && apt install openjdk-8-jre -y
wget https://github.com/find-sec-bugs/find-sec-bugs/releases/download/version-1.9.0/findsecbugs-cli-1.9.0-fix2.zip && unzip findsecbugs-cli-1.9.0-fix2.zip -d /opt/
sed -i -e 's/\r$//' /opt/findsecbugs.sh
chmod +x /opt/findsecbugs.sh
export PATH=/opt/:$PATH
findsecbugs.sh -h
```
##### njsscan NOT FINISHED
```
pip3 install njsscan==0.3.3
pip3 install --upgrade urllib3
njsscan --json -o output.json .

```

##### pylint
```
pip3 install pylint
# Reduce false positives create a pylintrc file Example:
cat > .pylintrc <<EOF [MASTER] disable=missing-module-docstring,import-error EOF
```
##### Brakeman
```
apt update
apt install ruby-full -y
gem install brakeman -v 5.2.1
```

##### Sonarqube
```
export SONAR_VERSION="4.7.0.2747"
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_VERSION}-linux.zip -O /opt/sonar-scanner.zip
unzip /opt/sonar-scanner.zip -d /opt/
chmod +x /opt/sonar-scanner-${SONAR_VERSION}-linux/bin/sonar-scanner
export PATH=/opt/sonar-scanner-${SONAR_VERSION}-linux/bin/:$PATH

# LOGIN TO SONARQUBE GET TOKEN: https://sonarqube-af8tqyuu.lab.practical-devsecops.training/sessions/new

export SONARQUBE_TOKEN=INSERT_YOUR_TOKEN_HERE
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt install nodejs -y
sonar-scanner -Dsonar.projectKey=Django -Dsonar.sources=. -Dsonar.host.url=https://sonarqube-af8tqyuu.lab.practical-devsecops.training -Dsonar.login=$SONARQUBE_TOKEN

```

##### Nikto 
```
apt install -y libnet-ssleay-perl
git clone https://github.com/sullo/nikto
cd nikto/program
./nikto.pl -Help
```
##### NMAP
```
apt-get update && apt-get install nmap -y
nmap -help
```
##### SSLyze
```
pip3 install sslyze==5.0.3
sslyze --help
sslyze --json_out sslyze-output.json prod-af8tqyuu.lab.practical-devsecops.training:443
```
##### ZAP
```
docker pull owasp/zap2docker-stable:2.10.0
docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py --help
docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-af8tqyuu.lab.practical-devsecops.training

docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-af8tqyuu.lab.practical-devsecops.training -J zap-output.json
```
##### Dastardly
```
docker pull public.ecr.aws/portswigger/dastardly

docker run --user $(id -u) --rm -v $(pwd):/dastardly -e DASTARDLY_TARGET_URL=https://prod-af8tqyuu.lab.practical-devsecops.training -e DASTARDLY_OUTPUT_FILE=/dastardly/dastardly-report.xml public.ecr.aws/portswigger/dastardly
```
##### Ansible
```
pip3 install ansible==6.4.0
```
##### TFLint
```
curl https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash
git clone https://gitlab.practical-devsecops.training/pdso/terraform.git
cd terraform
tflint -h
```
##### Checkov
```
pip3 install checkov==2.1.50
```
##### Terrascan
```
wget https://github.com/accurics/terrascan/releases/download/v1.12.0/terrascan_1.12.0_Linux_x86_64.tar.gz
tar -xvf terrascan_1.12.0_Linux_x86_64.tar.gz
chmod +x terrascan
mv terrascan /usr/local/bin/
```
##### TFSec
```
wget -O /usr/local/bin/tfsec https://github.com/aquasecurity/tfsec/releases/download/v0.55.0/tfsec-linux-amd64
chmod +x /usr/local/bin/tfsec

```
##### Inspec
```
wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb
dpkg -i inspec_4.37.8-1_amd64.deb
inspec --help
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-af8tqyuu -i ~/.ssh/id_rsa --chef-license accept
```
