# Instructions
1.  Clone this repo, CD to local directory to where this repo has been cloned.
2.  Run docker-compose up -d
3.  Execute container as root:
	   - docker exec -it -u root jenkins-blueocean bash
4.  Execute the following command to give docker.sock the owner permission: 
	   - chown 1000:1000 /var/run/docker.sock
5.  Open browser and go to http://localhost:8080. Wait until the Unlock Jenkins page appears. Unlock Jenkins, install plugins and set up admin-user.
6.  Unzip ngrok-archive. CD to folder. Execute command the following command to set up public URL to http://localhost:8080. Add this URL to Github repository for sample application:
	   - ngrok http 8080
6.  Create job "BuildAndDeploy" (NOTE! Don't use spaces!), choose "Freestyle project".
7.  Choose Git as build trigger. Set repo https://github.com/ggoyal23/BTH_Demo.git and build from */main
8.  Add these commands in Build step ("Execute shell") to clean up Docker images and containers for sample application:
       - cd $PWD
       - docker ps
       - docker-compose stop
       - docker container prune --filter "label=com.docker.compose.project=buildanddeploy" -f
       - docker rmi -f buildanddeploy_app
	   - docker rmi -f mongo
       - docker volume prune --filter "label=com.docker.compose.project=buildanddeploy" -f
       - docker-compose up -d
9.  Add these commands in Build step ("Execute shell") to create Selenium grid:
	   - cd /var/jenkins_home/workspace 
	   - rm -rf Selenium_Java 
	   - git clone https://github.com/ggoyal23/Selenium_Java.git 
	   - cd Selenium_Java 
	   - docker-compose up -d
10. Add webhook in Github repository https://github.com/ggoyal23/BTH_Demo.git. Owner-permission required.
	   - Payload URL: https://{jenkins-url:port}/github-webhook/
	   - Content type: application/json
	   - Disable SSL certification
	   - Trigger only on push events
	   - In Jenkins job:
			- Select Git, add repo url in source code management, build from */main
			- Select Build triggers, then select "GitHub hook trigger for GITScm polling"
11. Create job "Test". Choose "Pipeline project" and add Github repo https://github.com/ggoyal23/Selenium_Java.git. Build from */master