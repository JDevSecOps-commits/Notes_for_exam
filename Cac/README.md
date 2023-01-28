get Inspec
wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb  
dpkg -i inspec_4.37.8-1_amd64.deb  

Prevent SSH Agent from prompting YES or NO  
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config  

Run Inspec against the production server
inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-xioqjd4c -i ~/.ssh/id_rsa --chef-license accept  

-t = target machine
-i is the ssh-key since login is via ssh
--chef-license tells that we are accepting license, prevents asking YES/NO

Example PCI/DSS checks
```
cat > /inspec-profile/challenge/controls/example.rb <<EOL
describe file('/etc/pam.d/system-auth') do
    its('content') { 
        should match(/^\s*password\s+requisite\s+pam_pwquality\.so\s+(\S+\s+)*try_first_pass/)
    }
    its('content') {
        should match(/^\s*password\s+requisite\s+pam_pwquality\.so\s+(\S+\s+)*retry=[3210]/)
    }
end

describe file('/etc/pam.d/password-auth') do
    its('content') { 
        should match(/^\s*password\s+requisite\s+pam_pwquality\.so\s+(\S+\s+)*try_first_pass/)
    }
    its('content') {
        should match(/^\s*password\s+requisite\s+pam_pwquality\.so\s+(\S+\s+)*retry=[3210]/)
    }
end

describe file('/etc/default/useradd') do
    its('content') {
        should match(/&\s*INACTIVE\s*=\s*(30|[1-2][0-9]|[1-9])\s*(\s+#.*)?$/)
    }
end

describe file('/etc/ssh/sshd_config') do
    it { should exist }
    it { should be_file }
    it { should be_owned_by 'root' }
    its('content') { should match 'PasswordAuthentication no' }
end
EOL
```
COntrols from linux optional lab

```
cat >> ubuntu/controls/configure_sudo.rb <<EOL
control 'ubuntu-1.3.1' do
   title 'Ensure sudo is installed'
   desc 'sudo allows a permitted user to execute a command as the superuser or another user, as specified by the security policy.'
   describe package('sudo') do
      it { should be_installed }
   end
end

control 'ubuntu-1.3.2' do
   title 'Ensure sudo commands use pty'
   desc 'Attackers can run a malicious program using sudo, which would again fork a background process that remains even when the main program has finished executing.'
   describe command('grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers').stdout do
      it { should include 'Defaults use_pty' }
   end
end

control 'ubuntu-1.3.3' do
   title 'Ensure sudo log file exists'
   desc 'Attackers can run a malicious program using sudo, which would again fork a background process that remains even when the main program has finished executing.'
   describe command('grep -Ei "^\s*Defaults\s+logfile=\S+" /etc/sudoers').stdout do
      it { should include 'Defaults logfile=' }
   end
end
EOL
```
ASVS controls
```
cat > ubuntu/controls/asvs.rb <<EOL
control 'ASVS-14.4.1' do
    impact 0.7
    title 'Safe character set'
    desc 'HTTP response contains content type header with safe character set'
    describe http('https://prod-xioqjd4c.lab.practical-devsecops.training') do
        its ('headers.Content-type') { should cmp 'text/html; charset=utf-8'}
    end
end

control 'ASVS-14.4.2' do
    impact 0.7
    title 'Contain Content Disposition header attachment'
    desc "Add Content-Disposition header to the server's configuration.","Add 'attachment' directive to the header."
    describe http('https://prod-xioqjd4c.lab.practical-devsecops.training') do
        its ('headers.content-disposition') { should cmp 'attachment' }
    end
end

control 'ASVS-14.4.3' do
    impact 0.7
    title 'Content Security Policy Options != none / contain unsafe-inline;unsafe-eval;\* '
    desc "Ensure that CSP is not configured with the directives: 'unsafe-inline', 'unsafe-eval' and wildcards."
    describe http('https://prod-xioqjd4c.lab.practical-devsecops.training') do
        its ('headers.content-security-policy') { should_not cmp 'none' }
        its ('headers.content-security-policy') { should_not include 'unsafe-inline;unsafe-eval;\*'}
    end
end

control 'ASVS-14.4.4' do
    impact 0.7
    title 'Content type Options = no sniff'
    desc 'All responses should contain X-Content-Type-Options=nosniff'
    describe http('https://prod-xioqjd4c.lab.practical-devsecops.training') do
        its ('headers.x-content-type-options') { should cmp 'nosniff'}
    end
end

control 'ASVS-14.4.5' do
    impact 0.7
    title 'HSTS is using directives max-age=15724800'
    desc 'Verify that HTTP Strict Transport Security headers are included on all responses and for all subdomains, such as Strict-Transport-Security: max-age=15724800; includeSubDomains.'
    describe http('https://prod-xioqjd4c.lab.practical-devsecops.training') do
        its ('headers.Strict-Transport-Security') { should match /\d/ }
    end
end

control 'ASVS-14.4.6' do
    impact 0.7
    title "'Referrer-Policy' header is included"
    desc "HTTP requests may include Referrer header, which may expose sensitive information. Referrer-Policy restiricts how much information is sent in the Referer header."
    describe http('https://prod-xioqjd4c.lab.practical-devsecops.training') do
        its ('headers.referrer-policy') { should cmp 'no-referrer; same-origin' }
    end
end
EOL
```



