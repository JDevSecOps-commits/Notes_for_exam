You can provide executable permissions to a file using the chmod command
```
cat > myfile <<<EOL
ls
EOL
```

verify contents with `cat`
Verify permissions using `ls -l`
Verify permissions using `stat myfile`
Give executable permissions using chmod `chmod +x myfile`
run a file using `./myfile`

`sudo` command to run with root privileges

create a new user
`echo -e "pdevsecops\npdevsecops" | adduser --gecos "" john

usermod command to add user to sudo group
`usermod -aG sudo john`

become a user
`sudo su - john`
