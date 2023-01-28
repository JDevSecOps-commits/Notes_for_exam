#### Command Resource

`grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*`
Checks for use_pty in sudoers file. Isn't found. To add to the file.

To run a command against a system. Use the command resource
```
cat > ubuntu/controls/example.rb <<EOL
control 'ubuntu-1.3.2' do
   title 'Ensure sudo commands use pty'
   desc 'Attackers can run a malicious program using sudo, which would again fork a background process that remains even when the main program has finished executing.'
   describe command('grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*') do
      its('stdout') { should match /Defaults(\s*)use_pty/ }
   end
end
EOL
```
The \s* matches any whitespace  

Useful Regex Site:  
https://rubular.com/  

Run locally 
`inspec check ubuntu`
`inspec exec ubuntu` 
Run remotely 
`echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config`  
`inspec exec ubuntu -t ssh://root@prod-xioqjd4c -i ~/.ssh/id_rsa --chef-license accept`  

#### File Resource
Use file resource for the previous control
```
cat > ubuntu/controls/example.rb <<EOL
control 'ubuntu-1.3.2' do
   title 'Ensure sudo commands use pty'
   desc 'Attackers can run a malicious program using sudo, which would again fork a background process that remains even when the main program has finished executing.'
   describe file('/etc/sudoers') do
      its('content') { should match /Defaults(\s*)use_pty/ }
   end
end
EOL
```
Can execute remotely the same way.

#### Service Resource
Tests whether an installation of a named service is successful and enabled

#### Command Resource 
Test an arbitrary command that is run on the system. Can read output etc
