Cerate a new engagement, login to DefectDojo

- Add product in sidebar
- Enter product details (Name, product type etc)
- Engagements > Add New interactive Engagement
- Fill in the details

Upload script, also will be in this repo 
`curl https://gitlab.practical-devsecops.training/-/snippets/3/raw -o upload-results.py`
`pip3 install requests`  

Options for DefectDojo:
- host: e.g.          https://dojo-xioqjd4c.lab.practical-devsecops.training/ 
- Username:           i.e root
- API key. Find at:   https://dojo-xioqjd4c.lab.practical-devsecops.training/api/key-v2
- ENGAGEMENT_ID:      ID of engagement, here it's 1
- PRODUCT_ID:         ID of product, here it's 1
- LEAD_ID:            ID of the user conducting the testing
- ENVIRONMENT:        Environment name
- SCANNER:            Name of the scanner CASE SENSITIVE, e.g. ZAP Scan
- RESULT_FILE:        The path to the tool's output

Get the API key and set to environment variable (change the defectdojo machine name)
`export API_KEY=$(curl -s -XPOST -H 'content-type: application/json' https://dojo-xioqjd4c.lab.practical-devsecops.training/api/v2/api-token-auth/ -d '{"username": "root", "password": "pdso-training"}' | jq -r '.token' )`

Example uploading of the script. Change server, engagement id product id , tool, filename (maybe lead id ) 
`python3 upload-results.py --host dojo-xioqjd4c.lab.practical-devsecops.training --api_key $API_KEY --engagement_id 2 --product_id 3 --lead_id 1 --environment "Production" --result_file brakeman-result.json --scanner "Brakeman Scan"`

Can mark false positive by clicking an issue then clicking bulk edit. Then choose criteria for doing so.  

#### Vuln management example
First get the results output 
`bandit -r . -f json | tee bandit-output.json`  
Get the upload script 
`curl https://gitlab.practical-devsecops.training/-/snippets/3/raw -o upload-results.py`  
Upload the scan using the upload script
`python3 upload-results.py --host dojo-xioqjd4c.lab.practical-devsecops.training --api_key $API_KEY --engagement_id 1 --product_id 1 --lead_id 1 --environment "Production" --result_file bandit-output.json --scanner "Bandit Scan"`

Zap command in xml
`docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw \
           --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py \
           -t https://prod-xioqjd4c.lab.practical-devsecops.training \
           -d -x zap-output.xml`
Upload the scan results
`python3 upload-results.py --host dojo-xioqjd4c.lab.practical-devsecops.training --api_key $API_KEY --engagement_id 1 --product_id 1 --lead_id 1 --environment "Production" --result_file zap-output.xml --scanner "ZAP Scan"`


