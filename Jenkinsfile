pipeline {
  agent any

  tools {
        go 'go-1-19'
    }
    environment {
        GO111MODULE = 'on'
    }
   
  stages {

        stages {
        stage('Compile') {
            steps {
                sh 'go build'
            }
        }
        stage('Test') {
            steps {
                sh 'go test ./... -coverprofile=coverage.txt'
                sh "curl -s https://codecov.io/bash | bash -s -"
            }
        }
     

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
