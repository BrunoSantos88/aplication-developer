pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials-id')
    }
    
 stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t brunosantos88/aplicationdeveloper:1 .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub using credentials
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub-credentials-id', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                        sh "docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
                        
                        // Push the Docker image to Docker Hub
                        sh 'docker push brunosantos88/aplicationdeveloper:1'
                    }
                }
            }
        }
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
