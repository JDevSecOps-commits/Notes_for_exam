### Inspec Shell
`mkdir inspec-profile && cd inspec-profile`

Initialise an inspec profile
`inspec init profile ubuntu --chef-licence accept`

```
cat >> ubuntu/controls/example.rb <<EOL
describe file('/etc/shadow') do
    it { should exist }
    it { should be_file }
    it { should be_owned_by 'root' }
  end
EOL
```

`inspec check ubuntu`
`inspec exec ubuntu`

Execute the profile on a remote server
`inspec exec ubuntu -t ssh://root@prod-xioqjd4c -i ~/.ssh/id_rsa --chef-license accept`

#### Inspec Shell
Go into the shell  
`inspec shell -t ssh://root@prod-xioqjd4c -i ~/.ssh/id_rsa --chef-license accept`  
Look for resources  
`help resources`
Use Ruby syntax to retrieve out methods available to the file resource   
`file('/tmp').class.superclass.instance_methods(false).sort`  
Can scroll through the list with up and down arrows, Q to quit  
Check if a method exists  
`file('/tmp').directory?`  
`file('/tmp').content`  
Can type in console to test
```
describe file('/tmp') do
it { should be_directory }
end
```
Use os_env to test the environment variables
```
os_env('Path')
os_env('Path').content
os_env('Path').split
```
type exit to exit the session

