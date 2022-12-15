# DevSecOps-Pro
Notes for future / Exam
>Install Safety Tool
```bash
pip3 install safety==2.1.1
```

>Help for safety tool
```bash
safety check --help
```

> Run the scanner
```bash 
safety check -r requirements.txt --json | tee safety-output.json
```
