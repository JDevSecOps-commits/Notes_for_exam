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
> Useful hints
```
-r flag used to specify the input

--json flag tells that output should be in JSON format

- docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
```
>Fix
```
cat >requirements.txt<<EOF
Django~=2.2.25
EOF
```

> Retire js
```
--outputpath : flag specifies the output file path.
--outputformat : flag specifies that output format. Here it’s the JSON format.

```

> Ignore file
```
retire --severity high --ignorefile .retireignore.json --outputformat json --outputpath retire_output.json

```
> retire js
```
install retire js
npm install -g retire
