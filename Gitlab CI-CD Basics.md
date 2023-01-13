# Git
```
apk add git - installs git
git-status
git config --global user.email "student@pdevsecops.com"
git config --global user.name "student"
git clone http://root:pdso-training@gitlab-ce-xcs30z62.lab.practical-devsecops.training/root/django-nv.git
curl https://gitlab.practical-devsecops.training/-/snippets/3/raw -o upload-results.py
git add upload-results.py
git commit -m "Add upload-results.py file"
```

# GitLab CI/CD Advanced
To Fail a build
```bash
- exit 1 - fails a build
allow_failure: true (allows the build to fail)
```

# Save the results
```bash
artifacts:                    # <--- To save results, we use artifacts tag
    paths:                      # <--- We then give the path/paths of the scan result files we want to store for further processing
    - vulnerabilities.json                  #<--- The filename
    expire_in: 1 week 
 ```
# Continous Delivery
```bash
when: manual
```

# Conditional Pipelines
```bash


# YAML File
```bash
# This is how a comment is added to a YAML file; please read them carefully.

stages:         # Dictionary
 - build        # this is build stage
 - test         # this is test stage
 - integration  # this is an integration stage
 - prod         # this is prod/production stage

job1:
  stage: build  # this job belongs to the build stage.
  script:
    - echo "This is a build step."  # We are running an echo command, but it can be any command.

job2:
  stage: test
  script:
    - echo "This is a test step."
    - exit 1          # Non zero exit code, fails a job.

job3:          
  stage: integration  # integration stage
  script:
    - echo "This is an integration step."

job4:
  stage: prod
  script:
    - echo "This is a deploy step."
```
