# solution

### 1) Dockerization
in the beginning, i go to GO official image on dockerhub and get the recommended dockerfile. i tried to build it locally and run a container from output image but the container continues to crash. i edit entry point to be sleep 3600 and get into the container and ran the application, i get that ".env" and "users.json" files does not exist in the container. then i found out that they are in ".dockerignore" file so, COPY command ignores them then i removed them from ".dockerignore" file. i also noticed that the port of the application is 4444 so, i exposed container on port 4444. and finally it worked locally.

### 2) CICD
the problem that takes too much time from me in CI process is that i need jenkins with specific features:
    a) jenkins has to have docker installed on it to build docker file and push output image to dockerhub. 
     - So i customize jenkins base image and install docker on it. you will find it in "jenkins_image_with_docker_installed" directory.
    b) jenkins has to be exposed on the internet so that i can create a webhook from project github repository. 
     - So i launched an EC2 instance and deploy jenkins container from customized image from step 1 
     - " http://18.212.180.3:2020/ " this is a url to access jenkins. it might be down by the time you are reviewing this task accoeding to its cost. 
    C) jenkins has to have Go installed on it to run required tests.
     - So i install Go plugin on jenkins and configured it and used it in jenkinsfile.
After I had finished deploying jenkins with all dependencies needed, i configure jenkinsfile with all needed stages:
    a) first stage is to run needed tests. 
    b) second stage is to build dockerfile and push output image to dockerhub. 
Finally, i configured jenkins pipeline from jenkins ui to be triggered from github webhook. 

### 3) Kubernetes
here i used previously installed EKS cluster to deploy Chat application on it as pod with its service. as in "deployment.yaml" file i configure deployment to run from the image i already pushed to dockerhub with one replica. i also configured a NodePort service expose this deployment to the internet out of the cluster and later to be pointed by a load balancer.


### 4) Resilience
to acheive reselience i need to make a pv and pvc objects and attch it to EFS. and that needs efs csi driver which is not installed on the cluster i used. 

### 5) Expose out of cluster
Finally I configure an application load balancer ingress which provisions new aws alb to redirect its traffic to the Nodeport service and finally to the pod. 
this is the dns name of the alb which we can later point it to real domain in route53 and create certificate to it to be secure on https: 
  k8s-default-chatting-bdace69f06-321822523.us-east-1.elb.amazonaws.com
