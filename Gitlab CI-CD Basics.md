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
