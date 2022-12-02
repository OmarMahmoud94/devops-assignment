# FROM jenkins/jenkins:lts
# USER root
# RUN apt-get update -qq  
# RUN apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common
# RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
# RUN add-apt-repository \
#        "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
# RUN apt-get update -qq && apt-get install -y docker-ce=17.12.1~ce-0~debian 
# RUN usermod -aG docker jenkins


FROM jenkins/jenkins:2.375.1-jdk11
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean:1.25.8 docker-workflow:521.v1a_a_dd2073b_2e"