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
