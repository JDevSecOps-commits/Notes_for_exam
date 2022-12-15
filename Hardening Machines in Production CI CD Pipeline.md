# DevSecOps-Pro
Notes for future / Exam

> Install Ansible
```bash
pip3 install ansible==4.7.0
```

> Create inventory
```bash
cat > inventory.ini <<EOL

# DevSecOps Studio Inventory
[devsecops]
devsecops-box-eggkd08x

[gitservers]
gitlab-ce-eggkd08x

[prod]
prod-eggkd08x
EOL
```

> use ssh-keyscan to capture to capture the signatures beforehand
```bash
ssh-keyscan -t rsa prod-eggkd08x >> ~/.ssh/known_hosts
```

> do the same to rest of the systems
```bash
ssh-keyscan -t rsa gitlab-ce-eggkd08x >> ~/.ssh/known_hosts
ssh-keyscan -t rsa devsecops-box-eggkd08x >> ~/.ssh/known_hosts
```
> you can do the same command as above
```bash
ssh-keyscan -t rsa prod-eggkd08x gitlab-ce-eggkd08x devsecops-box-eggkd08x >> ~/.ssh/known_hosts
```
> We will choose the dev-sec.os-hardening role from the dev-sec project to harden our production environment.
```bash
ansible-galaxy install dev-sec.os-hardening
```

> create playbook to run 
```bash
cat > ansible-hardening.yml <<EOL
---
- name: Playbook to harden Ubuntu OS.
  hosts: prod
  remote_user: root
  become: yes

  roles:
    - dev-sec.os-hardening

EOL
```
> run the playbook on production to harden pipeline
```bash
ansible-playbook -i inventory.ini ansible-hardening.yml
```


