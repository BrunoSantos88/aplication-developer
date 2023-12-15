pipeline {
  agent any

        environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerlogin')
    }

 stages {   

    stage('Docker Build') {
     steps {
      sh 'docker build -t brunosantos88/aplicationdeveloper .'
     }
   }

   stage('Docker Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
   
    stage('Docker Push') {
     steps {
       sh 'docker push brunosantos88/aplicationdeveloper:latest'
     }
   }


post {
        success {
            emailext subject: 'Build Success Notification',
                      body: 'The build was successful. Congratulations!',
                      recipientProviders: [culprits(), developers()],
                      to: 'brunosantosc1@gmail.com',
                      from: 'jenkins@example.com',
                      attachLog: true
        }
        failure {
            emailext subject: 'Build Failure Notification',
                      body: 'The build failed. Please check the Jenkins logs for details.',
                      recipientProviders: [culprits(), developers()],
                      to: 'brunosantosc1@gmail.com',
                      from: 'jenkins@example.com',
                      attachLog: true
        }
    }

}

}