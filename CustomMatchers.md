#### Example control

Ensure only root can change ssh config  
check permission bits 
`stat /etc/ssh/sshd_config`  
Change the files ownership and permissions
`chown root:root /etc/ssh/sshd_config`  
`chmod og-rwx /etc/ssh/shd_config`
Do it in inspec
```
cat > ubuntu/controls/example.rb <<EOL
control 'ubuntu-5.2.1' do
   title 'Ensure permissions on /etc/ssh/sshd_config are configured'
   desc 'The /etc/ssh/sshd_configfile contains configuration specifications for sshd. The command below checks whether the owner and group of the file is root.'
   describe file('/etc/ssh/sshd_config') do
     its('owner') { should eq 'root'}
     its('group') { should eq 'root'}
     its('mode') { should cmp '0600' }
   end
end
EOL
```

