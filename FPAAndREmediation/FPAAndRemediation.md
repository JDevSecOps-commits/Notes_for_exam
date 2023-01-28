`pip3 install bandit==1.7.4`  
`bandit -r .`  

Two ways to do FPA, reading source code or exploiting the vuln

https://www.mysqltutorial.org/sql-concat-in-mysql.aspx/

look at cve, find the location in the file. See the method being called. Find the package being used, search the CVE libary.
Determine if false positive in this instance.
Keep it in the baseline. (the stats don't change) 
then run `bandit -r . --baseline=baseline.json` 

To remediate injection errors need to do string inserting (with the "%s",[first_var, second_var] etc.)

To remediate python yaml errors or other function problems swap to safe_load or other secure functions.

example baseline: 
https://gitlab.practical-devsecops.training/-/snippets/15  

#### Semgrep Custom rule
`...` operator used to match a sequence of zero or more arguments, statements or characters in a given code
os.system(...)
*Metavariables* can be used to track values across a specific code scope. THis inclused variables, functions, arguments, classes, object methods, imports, eceptions and more. Especially when we don't know the value or contents beforehand.
`$X = $Y` matches all x = y bits of code
`$X.objects.get` matches all objects.get objects

##### Rule syntax

Example rule
```
cat > command_injection.yaml <<EOF
rules:
- id: Possible Command Injection
  patterns:
  - pattern: os.system(...)
  - pattern-not: os.system("...") # Don't search ones that use hard coded commands
  message: Possible Command Injection
  languages:
  - python
  severity: WARNING
EOF
```
Following fields are required for a rule
id (string)	Unique, descriptive identifier, e.g., possible-command-injection  
message (string)	Message highlighting why this rule fired and how to remediate the issue, e.g. Command injection attack  
severity (string)	One of: WARNING, ERROR  
languages (array)	Any of: go, java, javascript, python, typescript  
pattern (string)	Find code matching this expression  
patterns (array)	Logical AND of multiple patterns  
pattern-either (array)	Logical OR of multiple patterns  
pattern-regex (string)	Search files for Python re compatible expressions  


pattern-inside example. Keeps findings that lie in the pattern.

```
cat > find_project_db_get_call.yaml <<EOF
rules:
- id: find-get-project-db-value
  patterns:
  - pattern: Project.objects.get(...)
  - pattern-inside: |
      def \$FUNC(request):
        ...
  message: Get project db value
  languages:
  - python
  severity: WARNING
EOF
```
Following fields can be used for filtering

metavariable-regex (map)	Search metavariables for Python re compatible expressions  
pattern-not (string)	Logical NOT - remove findings matching this expression   
pattern-inside (string)	Keep findings that lie inside this pattern  
pattern-not-inside (string)	Keep findings that do not lie inside this pattern  
pattern-where-python (string)	Remove findings matching this Python expression  


#### Extra FPA notes
Typically SAST traces from user input (sources) to critical security functions (sinks) 


