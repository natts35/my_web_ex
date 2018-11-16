pipeline {
    agent none
    environment {
        imageName = 'nootiew/my_web_ex'
        port = 80
    }
    
    stages {
       stage('Package') { 
          agent {label 'mgr1'}
          steps {
              sh "docker --version"
              sh "docker build -t ${imageName} ."
            withCredentials(
                [usernamePassword(credentialsId: 'docker_hub', 
                passwordVariable: 'dockerHubPassword', 
                usernameVariable: 'dockerHubUser')]) {
              sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
              sh "docker push ${imageName}"
            }
          }
       }

       stage('Deploy') { 
          agent {label 'mgr1'}
          steps {
           sh "docker service create --replicas 2 --name my_web_ex --constraint "node.role != manager" --publish 9090:80 ${imageName}:latest"
          }
       }
    }
}