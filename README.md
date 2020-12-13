# JenkinsRepo
1. Go to directory where repo has cloned.
2. Run Docker-compose file
3. Execute container as root 
   docker exec -it -u root jenkins bash
4. Execute following command to give the docker.sock the owner permission.
    chown 1000:1000 /var/run/docker.sock
 
 5. For Build&Deploy Jenkins job 
    a)add following commands in execute shell 
		  cd $PWD
		  docker ps
		  docker-compose stop
		  docker container prune --filter "label=com.docker.compose.project=buildanddeploy" -f
		  docker image prune
		  #docker rmi -f buildanddeploy
		  #docker rmi -f mongo
		  docker volume prune --filter "label=com.docker.compose.project=buildanddeploy" -f
		  docker-compose up -d
	b) add another execute shell command and add following lines:
		  cd /var/jenkins_home/workspace
		  rm -rf Selenium_Java
		  git clone https://github.com/ggoyal23/Selenium_Java.git
		  cd Selenium_Java
		  docker-compose up -d
      
6. For Test Job, create Pipeline project and add Selenium_Java Git Repo


