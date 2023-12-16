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
	
	
stage('Test') { //on this stage New container will be created, but current pipeline workspace will be remounted to it automatically
        agent {
            docker {
                image 'maven:latest'
                args '-v /root/.m2:/root/.m2'
            }
        }
        steps {
            sh 'mvn test' 
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

 }
}
