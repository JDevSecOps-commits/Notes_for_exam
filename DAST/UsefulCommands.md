docker pull public.ecr.aws/portswigger/dastardly  
docker run --user $(id -u) --rm \ -v $(pwd):/dastardly \ -e DASTARDLY_TARGET_URL=https://prod-xioqjd4c.lab.practical-devsecops.training \ -e DASTARDLY_OUTPUT_FILE=/dastardly/dastardly-report.xml \ public.ecr.aws/portswigger/dastardly  
