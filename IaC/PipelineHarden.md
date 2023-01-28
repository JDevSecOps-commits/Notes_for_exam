make an inventory  
download the role(s)  
make a playbook  
apply the playbook to the inventory  


Important link for hardening: https://github.com/dev-sec/ansible-collection-hardening  
lots of examples
ansible-galaxy install dev-sec.os-hardening

Putting Hardening in CI CD pipeline.  
Add DEPLOYMENT_SERVER to variables in gitlab 
Add DEPLOYMENT_SERVER_SSH_PRIVKEY 
ssh key available at /root/.ssh/id_rsa 

upload ansible-hardening.yml playbook to the git repo so they can run it
