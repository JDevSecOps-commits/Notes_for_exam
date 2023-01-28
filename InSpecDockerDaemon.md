Check docker benchmark on local machine  
`inspec exec https://github.com/dev-sec/cis-docker-benchmark --chef-license accept`  
IF container is not running, then some tests will be skipped 
Run a profile against a docker container:  
First create a running container e.g.  
`docker run -d --name alpine -it alpine /bin/sh`  
Exec against this container  
`inspec exec https://github.com/dev-sec/linux-baseline --chef-license accept -t docker://alpine`
Exec a baseline against the Docker daemon itself now that the container is running
`inspec exec https://github.com/dev-sec/cis-docker-benchmark --chef-license accept`  
This will now execute more tests against the daemon.  


