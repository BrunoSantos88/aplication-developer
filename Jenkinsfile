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

	
stage("install maven") {
      agent { 
        docker {
           label "maven" 
            image "maven:3.8-openjdk-8"
           }
           }
}
	
  // SonarCloud Scanner
    stage ('Build') {
    steps {
        echo 'This is a minimal pipeline.'
         sh 'mvn clean install'
    }
}
   
    stage('SonarCloud') {
            steps {	
		sh 'mvn clean verify sonar-scanner -Dsonar.organization=cloudsonarscan -Dsonar.projectKey=cloudsonarscan_brunosantos -Dsonar.sources=. -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=ce109f9d97e94fde13f38124ff5bcf2aa4adafac'
			}
        } 
	

 }
}
