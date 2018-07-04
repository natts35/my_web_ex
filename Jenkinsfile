pipeline {
    agent none
    environment {
        imageName = 'nootiew/my_web_ex'
        port = 80
    }
    
    stages {
       stage('Build') { 
          agent any
          steps {
              sh "docker --version"
              sh "docker build -t ${env.imageName} ."
          }
       }

       stage('Package') { 
          agent any
          steps {
            withCredentials(
                [usernamePassword(
                    credentialsId: 'docker_hub', 
                    passwordVariable: 'dockerHubPassword', 
                    usernameVariable: 'dockerHubUser'
                )]
            ){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh "docker push ${env.imageName}"
            }
          }
       }

       stage('Deploy') { 
          agent {label 'mgr1'}
          steps {
                sh "echo create service"
              }
          }
       }
    }
}