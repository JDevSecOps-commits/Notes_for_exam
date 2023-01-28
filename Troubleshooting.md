### Error 500 Defect Dojo. Check if lab problem

I had a similar problem uploading results to DefectDojo in the VM modules. There was an error 500 message stating that there was an argument missing and therefore the upload had failed. This is an error discussed in the labs, however it isn't fixable. The real reason was because the production machine had not linked properly for whatever reason and therefore the solution proposed didn't fix the error. The DevSecOps staff just advised me to restart the lab if this happened because they were unable to find a solution  

It did, but I had to restart it twice for some reason for it to work. It was annoying as the error message is the same for different problems, like if you use the wrong engagement id or if you upload it in the wrong format. 

The way that the staff told me to check if it is a machine problem is to log in to DefectDojo and then try to add the scan manually. In the environment variable there should be a "Production" listed. If there isn't then it means that there is a problem with the lab and you need to restart

#### Tarik's working command
```
python3 upload-results.py --host dojo-94w9xh5v.lab.practical-devsecops.training \ --api_key $API_KEY --engagement_id 1 \ --product_id 1 --lead_id 1 \ --environment "Production" \ --result_file bandit-output.json \ --scanner "Bandit Scan"
```

#### Marisa's working command
I also had a DefectDojo question or two. Here's an example of the code I used in GitLab, which worked correctly: 

```
after_script:
- python3 upload-results.py --host $DOJO_HOST --api_key $DOJO_API_TOKEN --engagement_id 1 --product_id 1 --lead_id 1 --environment "Production" --result_file zap-output.xml --scanner "ZAP Scan"
```
And here is equvalent in DevSecOps box:  
```
$ python3 upload-results.py --host dojo-xxxxxxxx.lab.practical-devsecops.training \
--api_key $API_KEY --engagement_id 1 --product_id 1 \
--lead_id 1 --environment "Production" \
--result_file zap-output.xml \
--scanner "ZAP Scan"
```

### Other problems
