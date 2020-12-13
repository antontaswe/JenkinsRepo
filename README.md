# JenkinsRepo
1. Go to directory where repo has cloned.
2. Run Docker-compose file
3. Execute container as root 
   docker exec -it -u root jenkins bash
4. Execute following command to give the docker.sock the owner permission.
 - chown 1000:1000 /var/run/docker.sock
