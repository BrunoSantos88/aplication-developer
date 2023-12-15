pipeline {
    agent any
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Define the Docker image name and tag
                    def dockerImage = 'brunosantos88/aplicationdeveloper:01'
                    docker.build(dockerImage, '-f Dockerfile .')
                    docker.withRegistry('https://registry.example.com', 'docker-credentials-id') {
                    dockerImage.push()
                  }
                }
            }
        }
    }
}