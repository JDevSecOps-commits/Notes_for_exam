# DevSecOps-Pro
Notes for future / Exam
> show a text document on a terminal.
```bash
cat
``` 
# Log into a remote machine using ssh protocol.
```bash
ssh
``` 
# Change the file permissions(read, write and execute).
```bash
chmod
``
# Download, Upload and copy a git repository.
'''bash 
git (git pull, git push, git add, git clone) 
```
# Change directory, cd django.nv takes you into a django.nv folder.
```bash
cd
```
# Become another user (here deploy_user).
```bash
sudo su - deploy_user 
```
# Output/echo a string/number on a terminal.
```bash
echo 
```
# Exit a program with exit code as 1, usually fails a program when used.
```bash
exit 1 
```
# Used to create a directory.
```bash
mkdir
```
#  An easy to use Text editor.
```bash
nano
```

# Save output to file
```bash
cat /etc/passwd > mypasswd.txt
```

# Append to exisitng file (add new content to file)
```bash
cat /etc/passwd >> mypasswd.txt
```

# Extract content from file
```bash
cat mypasswd.txt | cut -d ':' -f 1
Behind the scenes, the cat command’s output was sent to the cut command as an input, and it used -d (delimiter) flag with a colon as a separator/delimiter and -f (field) option to get the 1st field.

-d - delimter flag 
-f - field (to get first field)
```

# Exit Codes
```bash
The exit code returned by a program could be obtained through echo $?
```

# Exit Codes with Security Tools
```bash
In the context of security tooling, unsuccessful execution generally means that the security tool found vulnerabilities.

In many cases, a mature security tool would return an exit code of zero indicating that the tool found no vulnerabilities.
A mature security tool would return a non zero exit code indicating that the tool found one or more security vulnerabilities.
```

# Git Basics
```bash
Configure Username:-
git config --global user.email "student@pdevsecops.com"
git config --global user.name "student"

Download repository
git clone http://root:pdso-training@gitlab-ce-xcs30z62.lab.practical-devsecops.training/root/django-nv.git

Add file to respsitory
git add myfile README.md

Commit changes to repo
git commit -m "Add myfile and update README.md"

Pull
git pull - pull/download repo to local repo

Push
git push - upload/push local repo to repo
```
# SSH Basics
```bash
ssh -o "StrictHostKeyChecking=no" -i ~/.ssh/id_rsa root@prod-xcs30z62
```
Add keys to known_hosts file
```bash
ssh-keyscan -t rsa prod-xcs30z62 >> ~/.ssh/known_hosts
```

# jq
```bash
jq '.' learnjq.json

jq '.[] | .details' learnjq.json
jq '.[] | {name: .details.name, url:.details.url}' learnjq.json
jq '.[1] | {name: .details.name, url: .details.url}' learnjq.json
jq '[.[] | {name: .details.name, url: .details.url}]' learnjq.json
jq '.[] | {name: .details.name, url: .details.url, services: .services[]}' learnjq.json
jq '.[] | {name: .details.name, url: .details.url, services: [.services[]]}' learnjq.json
jq '.[] | {name: .details.name, servicecount: .services | length}' learnjq.json
jq '.[] | select(.details.name=="AWS")' learnjq.json
cat bandit-output.json | jq .metrics._totals.'"CONFIDENCE.HIGH"'

