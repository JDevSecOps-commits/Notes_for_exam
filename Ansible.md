# DevSecOps-Pro
Notes for future / Exam

> install ansible program
```bash
pip3 install ansible==6.2.0
```

> create inventory file
```bash
cat > inventory.ini <<EOL

[devsecops]
devsecops-box-eggkd08x

[sandbox]
sandbox-eggkd08x

[prod]
prod-eggkd08x

EOL
```
> To see which hosts in our inventory matches a supplied group name, let’s try the following command.
```bash
ansible -i inventory.ini prod --list-hosts
```

> find available modules
```bash
ansible-doc -l
```

> Use the Ansible command module to find shell module
```bash
ansible-doc -l | grep shell
```

> Use inventory file (-i inventory.ini) and run the uptime command on the production machine using shell module
```bash
ansible -i inventory.ini prod -m shell -a "uptime"
```

> Let’s create a playbook to run against the production environment.
```bash 
cat > playbook.yml <<EOL
---
- name: Example playbook to install firewalld
  hosts: prod
  remote_user: root
  become: yes
  gather_facts: no
  vars:
    state: present

  tasks:
  - name: ensure firewalld is at the latest version
    apt:
      name: firewalld

EOL
```

> Let’s run this playbook against the prod machine.
```bash
ansible-playbook -i inventory.ini playbook.yml
```


