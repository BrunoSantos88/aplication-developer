pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerlogin')
  }
	
stages {
	
 stage('Checkout code') {
  steps {
  checkout scm
        }
    }
	
//Docker Versionamento
    stage('Docker Build') {
     steps {
      sh 'docker build -t brunosantos88/aplicationdeveloper:2.0 .'
     }
   }

   stage('Docker Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
   
    stage('Docker Push') {
     steps {
       sh 'docker push brunosantos88/aplicationdeveloper:2.0'
     }
   }

    stage('SonarCloud Scan Frontend') {
      agent {
        docker { image 'node:16-alpine' }
      }
      steps {
        sh 'node --version'
        sh 'npm install -g sonarqube-scanner -Dsonar.projectKey=cloudsonarscan_brunosantos -Dsonar.login=ce109f9d97e94fde13f38124ff5bcf2aa4adafac'
      }
    }
}	
}
