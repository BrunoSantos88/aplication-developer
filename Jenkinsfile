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

 stage('Docker-login') {
      steps {
        sh 'docker --version'
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }

  stage('Docker-build') {
      steps {
        sh 'docker build -t brunosantos88/aplicationdeveloper:3.0'


 stage('Docker-Push') {
      steps { 
        sh 'docker push brunosantos88/aplicationdeveloper:3.0'
      }
    }
}
}
