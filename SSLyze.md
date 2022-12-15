# DevSecOps-Pro
Notes for future / Exam

> Install SSlyze
```bash
pip3 install sslyze==5.0.3
```

> Install new version of python
```bash
add-apt-repository -y ppa:deadsnakes/ppa
apt install -y python3.7
```

> Set default version
```bash
update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1
```

> Run the scan 
```bash
sslyze --json_out sslyze-output.json prod-eggkd08x.lab.practical-devsecops.training:443
```

> check the scan
```bash
cat sslyze-output.json
```
