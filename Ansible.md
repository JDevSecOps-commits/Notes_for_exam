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
> Capture Key signatures to input into known_hosts file
```bash
ssh-keyscan -t rsa sandbox-z3l8hs55 devsecops-box-z3l8hs55 >> ~/.ssh/known_hosts
```
>Yaml File
```bash
cat > tasks/main.yml <<EOL
---
- name: Install nginx
  apt:
    name: nginx
    state: present
    update_cache: true

- name: Copy the configuration
  template:
    src: templates/default.j2
    dest: /etc/nginx/sites-enabled/default

- name: Start nginx service
  service:
    name: nginx
    state: started
    enabled: yes

- name: Clone django repository
  git:
    repo: https://gitlab.practical-devsecops.training/pdso/django.nv.git
    dest: /opt/django

- name: Install dependencies
  command: pip3 install -r requirements.txt
  args:
    chdir: /opt/django

- name: Database migration
  command: python3 manage.py migrate
  args:
    chdir: /opt/django

- name: Load data from the fixtures
  command: python3 manage.py loaddata fixtures/*
  args:
    chdir: /opt/django

- name: Run an application in the background
  shell: nohup python3 manage.py runserver 0.0.0.0:8000 &
  args:
    chdir: /opt/django
EOL
```

>Adjust remote machine
```bash
cat > templates/default.j2 <<EOL
{% raw %}
server {
    listen      80;
    server_name localhost;

    access_log  /var/log/nginx/django_access.log;
    error_log   /var/log/nginx/django_error.log;

    proxy_buffers 16 64k;
    proxy_buffer_size 128k;

    location / {
        proxy_pass  http://localhost:8000;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_redirect off;

        proxy_set_header    Host \$host;
        proxy_set_header    X-Real-IP \$remote_addr;
        proxy_set_header    X-Forwarded-For \$proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto http;
    }
}
{% endraw %}
EOL
```

>File to use main.yml
```bash
cat > main.yml <<EOL
---
- name: Simple playbook
  hosts: sandbox
  remote_user: root
  gather_facts: no

  tasks:
  - include: tasks/main.yml
EOL
```

>Create cfg in Ansible
```bash

mkdir /etc/ansible/


cat > /etc/ansible/ansible.cfg <<EOF
[defaults]
stdout_callback = yaml
deprecation_warnings = False
host_key_checking = False
retry_files_enabled = False
inventory = /inventory.ini
EOF
```

>Copy files
```bash
ansible -i inventory.ini all -m copy -a "src=/root/notes dest=/root"
```

>Install module via Ansible
```bash
ansible -i inventory.ini prod -m apt -a "name=ntp state=present"
```





>Ansible Hardening
```bash
cat > /hardening/ansible-hardening.yml <<EOL
---
- name: Playbook to harden Ubuntu OS.
  hosts: prod
  remote_user: root
  become: yes

  roles:
    - dev-sec.os-hardening

EOL
```

>Dev-Sec Hardening
```bash
ansible-galaxy install dev-sec.os-hardening

ansible-playbook -i /hardening/inventory.ini /hardening/ansible-hardening.yml
```

>Embed ansible into CI/CD Pipeline
```bash
ansible-hardening:
  stage: prod
  image: willhallonline/ansible:2.9-ubuntu-18.04
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -t rsa $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - echo -e "[prod]\n$DEPLOYMENT_SERVER" >> inventory.ini
    - ansible-galaxy install dev-sec.os-hardening
    - ansible-playbook -i inventory.ini ansible-hardening.yml
 ```
