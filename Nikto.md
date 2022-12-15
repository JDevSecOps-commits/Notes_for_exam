# DevSecOps-Pro
Notes for future / Exam


> Nikto 
```bash
apt install -y libnet-ssleay-perl
```

> Git clone 
```bash
git clone https://github.com/sullo/nikto
```

> Perfrom Scan in Nikto
```bash
./nikto.pl -output nikto_output.xml -h prod-eggkd08x
```

> Show nikto results in XML
```bash
cat nikto_output.xml
```

> Show Plugins
```bash
./nikto.pl -list-plugins
```

> Run dictionary plugins 
```bash
./nikto.pl -h prod-ol6iehm8 -Plugins "@@default;dictionary(dictionary:/nikto/program/databases/db_dictionary)"
```

> Configure Nikto (nikto.conf) to remove the ports that would never be scanned, remove the False Positives & save output to csv format
```bash
cat >/nikto/program/nikto.conf<<EOF
SKIPPORTS=21 22 111
SKIPIDS=00035 00045
CLIOPTS=-output result.csv -Format csv
EOF
```



