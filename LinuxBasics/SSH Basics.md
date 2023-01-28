`ssh -o "StrictHostKeyChecking=no" -i ~/.ssh/id_rsa user@hostname`
-i allows specify the private key to use to login into a remote machine

Add target host's fingerprint to the list of known hosts 
```
ssh-keyscan -t rsa prod-xioqjd4c >> ~/.ssh/known_hosts
```
find the hostname of the client you're on
`hostname`

run commands remotely via ssh
```
ssh user@hostname "command"
```

Enable password less login, using public private key pairs

add to `/etc/ssh/sshd_config`

```
cat >> /etc/ssh/sshd_config <<EOF
RSAAuthentication yes
PubKeyAuthentication yes
EOF
```

restart the ssh server
`/etc/ini.d/sshd restart`

generate new ssh keys for first time setup 
```
ssh-keygen -t rsa
```

default storage locations
private key: `~/.ssh/id_rsa`
public key: `~/.ssh/id_rsa.pub`

remote machine needs to know the public key from where the connection id originating from.

Let remote machine allow connections
```
ssh-copy-id -i ~/.ssh/id_rsa.pub user@targetserver
```
prompt for user password to compelte auth.

After auth, will read the public key from `~/ssh/id_rsa.pub`
and adds it to `~/ssh/authorized_keys` on targetserver

