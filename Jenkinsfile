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

 stage('Docker-Pipeline') {
      agent {
        docker { image 'docker:latest' }
      }
      steps {
        sh 'docker --version'
	sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        sh 'docker build -t brunosantos88/aplicationdeveloper:3.0'
        sh 'docker push brunosantos88/aplicationdeveloper:3.0'
      }
    }
}
}
