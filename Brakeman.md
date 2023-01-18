>Install brakeman
```bash
apt update
apt install ruby-full -y
gem install brakeman -v 5.2.1
```

>Run Brakeman
```bash
brakeman -f json -i brakeman.ignore | tee result.json
```

>Run Brakeman with brakeman ignore file
```bash
brakeman -f json -i brakeman.ignore | tee result.json
```
