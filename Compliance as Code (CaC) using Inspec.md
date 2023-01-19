# DevSecOps-Pro
Notes for future / Exam

> Download Inspec 
```bash
wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb
```

>install inspec
```bash
dpkg -i inspec_4.37.8-1_amd64.deb
```

> Before executing the profile, we need to run the below command:
```bash
Before executing the profile, we need to run the below command:
```
> Let’s run the Inspec against the production server.
```bash
inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-eggkd08x -i ~/.ssh/id_rsa --chef-license accept
```

> Use hysnsec/inspec docker image to perform continuous compliance scanning with https://github.com/dev-sec/linux-baseline profile and embed it as part of the prod stage with job name as inspec
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
 
 > The job should only be triggered when changes are pushed to the master branch (don’t use rules attribute)
 ```bash
 inspec:
  stage: prod
  only:
    - master
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

> Save the output as JSON file at /share/inspec-output.json and upload it using artifacts attribute
```bash
inspec:
  stage: prod
  only:
    - master
  environment: production
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -t rsa $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - docker run --rm -v ~/.ssh:/root/.ssh -v $(pwd):/share hysnsec/inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@$DEPLOYMENT_SERVER -i ~/.ssh/id_rsa --chef-license accept --reporter json:/share/inspec-output.json
  artifacts:
    paths: [inspec-output.json]
    when: always
    
 ```
 >Prevent SSH KEY
 ```bash
 echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
 ```
 
 >SSH Inspec
 ```bash
 inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-xcs30z62 -i ~/.ssh/id_rsa --chef-license accept
 ```
 
 >Create Inspec Profile
 ```bash
 inspec init profile ubuntu --chef-license accept
 ```
 
 >Edit Inspec profile
 ```bash
 cat >> ubuntu/controls/example.rb <<EOL
describe file('/etc/shadow') do
    it { should exist }
    it { should be_file }
    it { should be_owned_by 'root' }
  end
EOL
```

>Validate errors
```bash
inspec check ubuntu
```

>Run inspec profile against server
```bash
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
inspec exec ubuntu -t ssh://root@prod-xcs30z62 -i ~/.ssh/id_rsa --chef-license acceptinspec exec ubuntu -t ssh://root@prod-xcs30z62 -i ~/.ssh/id_rsa --chef-license accept
