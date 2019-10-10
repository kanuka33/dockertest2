pipeline {  
  environment {
    registry = "sreeharshav/dockerpushtesting"
    registryCredential = 'dockerhub'
  }  
  agent any  
  stages {
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
	stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
	stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.100:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://10.1.1.100:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 dockerImage'
            }
        }
	stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.1.100:8000'
          }
  }
}
}
