pipeline {
  agent any
  stages
    {
    stage('Git checkout') {
      steps {
        git changelog: false, poll: false, url: 'https://github.com/Anji399/union.git' 
      }
    }
    stage('Package') {
      steps {
        sh 'mvn package'
      }
    }
    stage('Build docker image') {
      steps {
        sh 'docker build -t mvpar/union:1.0 .'  
      }    
    }
    stage('Push docker image') {
      steps {
       withCredentials([string(credentialsId: 'sec', variable: 'doc')]) {
        sh "docker login -u mvpar -p ${doc}"
       }  
        sh 'docker push mvpar/union:1.0'
      }    
    }
    stage('Deploy docker-app on host') {
      steps {
        sshagent(['dockser']) {
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.42.64'
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.42.64 docker run -d -p 8080:8080 --name mpr mvpar/union:1.0'
        }
      }    
    }
  }    
}
