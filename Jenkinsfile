pipeline {
  agent any
  stages
    {
    stage('Git checkout') {
      steps {
        git changelog: false, poll: false, url: 'https://github.com/Anji399/canara.git' 
      }
    }
    stage('Package') {
      steps {
        sh 'mvn package'
      }
    }
    stage('Build docker image') {
      steps {
        sh 'docker build -t mvpar/canara:1.0.0 .'  
      }    
    }
    stage('Push docker image') {
      steps {
       withCredentials([string(credentialsId: 'dockpw', variable: 'dockhubpw')]) {
        sh "docker login -u mvpar -p ${dockhubpw}"
       }  
        sh 'docker push mvpar/canara:1.0.0'
      }    
    }
    stage('Deploy docker-app on host') {
      steps {
        sshagent(['dock']) {
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.7.20'
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.7.20 docker run -d -p 8081:8080 --name mpr mvpar/canara:1.0.0'
        }
      }    
    }
  }    
}
