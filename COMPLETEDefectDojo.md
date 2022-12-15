# DevSecOps-Pro
Notes for future / Exam



> Download repo
```bash
git clone https://gitlab.practical-devsecops.training/pdso/rails.git webapp
```

> cd in to folder
```bash
cd webapp
```

> We will use docker to run brakeman scanner on the system to perform static analysis.
```bash
docker run --rm -v $(pwd):/src hysnsec/brakeman -f json /src
```

> As we have learned in the DevSecOps Gospel, we would like to store the scan results in a JSON file. We are using the tee command here to show the output and store it in a file simultaneously.
```bash
docker run --rm -v $(pwd):/src hysnsec/brakeman -f json /src | tee brakeman-result.json
```
