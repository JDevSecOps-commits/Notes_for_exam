# DevSecOps-Pro
Notes for future / Exam

> Download the spec file
```bash
wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb
```

> Install the file 
```bash
dpkg -i inspec_4.37.8-1_amd64.deb
```
> Create a new folder 
```bash
mkdir inspec-profile && cd inspec-profile
```

> Create the ubuntu profile
```bash
inspec init profile ubuntu --chef-license accept
```

> Run the following command to append the inspec task to the file at ubuntu/controls/example.rb. If you wish, you can edit the file using nano or any text editor.
```bash
cat >> ubuntu/controls/example.rb <<EOL
describe file('/etc/shadow') do
    it { should exist }
    it { should be_file }
    it { should be_owned_by 'root' }
  end
EOL
```

> Lets validate
```bash
inspec check ubuntu
```

> Now run the profile on the local-machine before executing on the server.
```bash
inspec exec ubuntu
```

> Let’s try to run the custom profile created by us against the server. Before executing the profile we need to execute the below command to avoid being prompted with Yes or No when connecting to a server via ssh.
```bash
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
```

> Lets run spec with the following options
```bash
inspec exec ubuntu -t ssh://root@prod-eggkd08x -i ~/.ssh/id_rsa --chef-license accept
```
