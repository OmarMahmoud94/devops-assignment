pipeline {
  agent any
   
  stages {
     stage('Build') {
           steps {
                 withCredentials([usernamePassword(credentialsId: "docker_credentials", usernameVariable: "username", passwordVariable: "pass")]) {
                 sh 'docker build . -t omarquraah/go_app_image:lts'
                 sh 'docker login -u $username -p $pass '
                 sh 'docker push omarquraah/go_app_image:lts'
                 }    
           }
     }
  
     
  }
}
