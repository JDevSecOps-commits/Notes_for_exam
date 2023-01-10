 > Safety
``` bash
> Safety checks your installed dependencies for known security vulnerabilities
```

>Install Safety Tool
```bash
pip3 install safety==2.1.1
```

>Help for safety tool
```bash
safety check --help
```

> Run the scanner
```bash 
safety check -r requirements.txt --json | tee safety-output.json
```
> Useful hints
```
-r flag used to specify the input

--json flag tells that output should be in JSON format

- docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
```
>Fix
```
cat >requirements.txt<<EOF
Django~=2.2.25
EOF
```

> Retire js
```
--outputpath : flag specifies the output file path.
--outputformat : flag specifies that output format. Here it’s the JSON format.

```

> Ignore file
```
retire --severity high --ignorefile .retireignore.json --outputformat json --outputpath retire_output.json

```
> retire js
```
install retire js
npm install -g retire


>Git Trufflehog
```git-secrets:
  stage: build
  script:
    - docker run -v $(pwd):/src --rm hysnsec/trufflehog --repo_path /src file:///src --json | tee trufflehog-output.json
  artifacts:
    paths: [trufflehog-output.json]
    when: always  # What is this for?
    expire_in: one week
    
  ```
  
  >Bandit
  ```
  bandit -r . -f json | tee bandit-output.json
  ```
  
  >SAST
  ```
  sast:
  stage: build  # we moved this job from test stage to build stage, by replacing the text test to build
  script:
    # Download bandit docker container
    - docker pull hysnsec/bandit
    # Run docker container, please refer docker security course, if this doesn't make sense to you.
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
```
