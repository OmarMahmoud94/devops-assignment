# solution

### 1) Dockerization
in the beginning, i go to GO official image on dockerhub and get the recommended dockerfile. i tried to build it locally and run a container from output image but the container continues to crash. i edit entry point to be sleep 3600 and get into the container and ran the application, i get that ".env" and "users.json" files does not exist in the container. then i found out that they are in ".dockerignore" file so, COPY command ignores them then i removed them from ".dockerignore" file. i also noticed that the port of the application is 4444 so, i exposed container on port 4444. and finally it worked locally.

### 2) CICD
the problem that takes too much time from me in CI process is that i need jenkins with specific features:
    a) jenkins has to have docker installed on it to build docker file and push output image to dockerhub. 
     - So i customize jenkins base image and install docker on it. you will find it in "jenkins_image_with_docker_installed" directory.
    b) jenkins has to be exposed on the internet so that i can create a webhook from project github repository. 
     - So i launched an EC2 instance and deploy jenkins container from customized image from step 1 
    C) jenkins has to have Go installed on it to run required tests.
     - So i install Go plugin on jenkins and configured it and used it in jenkinsfile.
After I had finished deploying jenkins with all dependencies needed, i wrote jenkinsfile with all needed stages:
    a) first stage is to


