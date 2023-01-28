Create an inventory
```
cat > inventory.ini <<EOL
# DevSecOps Studio Inventory
[devsecops]
devsecops-box-xioqjd4c

EOL
```

`ansible-galaxy install dev-sec.os-hardening`

Create a playbook
```
cat > ansible-hardening.yml <<EOL
---
- name: Playbook to harden Ubuntu OS.
  hosts: devsecops
  remote_user: root
  become: yes

  roles:
    - dev-sec.os-hardening

EOL
```

Run the playbook against the inventory
```
ansible-playbook -i inventory.ini ansible-hardening.yml
```

Run a docker inspec check for linux baseline 
```
docker run --rm -v ~/.ssh:/root/.ssh -v $(pwd):/share hysnsec/inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@$DEPLOYMENT_SERVER -i /root/.ssh/id_rsa --chef-license accept --reporter json:/share/inspec-output.json
```
NOTE: Share directory should be used when using hysnsec/inspec image. Because it's a custom image adding another directory would not work when your are saving the inspec output


