pipeline {

  agent any
        environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerlogin')
tools { 
        maven 'Maven 3.3.9' 
        jdk 'jdk8' 
    }

 stages {   

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
}
