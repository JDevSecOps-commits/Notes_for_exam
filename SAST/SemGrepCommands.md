#### Writing custom semgrep rules
pip3 install semgrep==0.81.0  

`semgrep --lang python -e '$X = $Y' .` 
`semgrep --lang python -e '$X.objects.get' .`  
```
cat > command_injection.yaml <<EOF
rules:
- id: Possible Command Injection
  patterns:
  - pattern: os.system(...)
  - pattern-not: os.system("...")
  message: Possible Command Injection
  languages:
  - python
  severity: WARNING
EOF
```

Run the semgrep scan using the above yaml file:  
`semgrep --lang python -f command_injection.yaml .`  
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
`semgrep --lang python -f find_project_db_get_call.yaml .`  

#### Hunting Vuln using Semgrep
Example Semgrep file for finding a decorator and a function 
```
cat > csrf_hunting.yaml <<EOF
rules:
- id: possible-csrf
  patterns:
  - pattern-inside: | 
      @csrf_exempt
      def \$FUNC(\$X):
          ...
  message: |
    Possible CSRF
  languages:
  - python
  severity: WARNING

- id: no-csrf-middleware
  patterns:
  - pattern: MIDDLEWARE_CLASSES=(...)
  - pattern-not: MIDDLEWARE_CLASSES=(...,'django.middleware.csrf.CsrfViewMiddleware',...)
  message: |
    No CSRF middleware
  languages:
  - python
  severity: WARNING
EOF
```
Search for DEBUG=true flag    
```
cat > debug_enable.yaml <<EOF 
rules:
- id: debug-enabled
  patterns:
  - pattern: DEBUG=True
  message: |
    Detected Django app with DEBUG=True. Do not deploy to production with this flag enabled
    as it will leak sensitive information.
  metadata:
    cwe: 'CWE-489: Active Debug Code'
    owasp: 'A6: Security Misconfiguration'
    references:
    - https://blog.scrt.ch/2018/08/24/remote-code-execution-on-a-facebook-server/
  severity: WARNING
  languages:
  - python
EOF
```
Porting security tools rulesets
Load bandit ruleset for semgrep  
`semgrep --config "p/bandit" .`  

Detect insecure redirect
```
cat > insecure_redirect.yaml <<EOF
rules:
- id: CWE-601
  pattern: |
    return redirect(request.\$M.get(...))
  message: Insecure Redirect
  severity: WARNING
  languages:
  - python
EOF

semgrep -f insecure_redirect.yaml
```
#### How to fix the issues reported by semgrep
Just run the scan and fix lol






