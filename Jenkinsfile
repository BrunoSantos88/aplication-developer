pipeline {

  agent { //here we select only docker build agents
            docker {
                image 'maven:latest' //container will start from this image
	    }
        environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerlogin')
	}


stages {   

 stage('Checkout code') {
  steps {
  checkout scm
        }
    }
	
	
stage('Maven Test') { 
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
