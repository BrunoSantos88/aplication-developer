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
 }

  stage('Docker-build') {
      steps {
        sh 'docker build -t brunosantos88/aplicationdeveloper:3.0 .'
      }
  }

 stage('Docker-Push') {
      steps { 
        sh 'docker push brunosantos88/aplicationdeveloper:3.0'
      }
    }

	stage('Kubernetes Deployment of ASG Bugg Web Application') {
	   steps {
	      withKubeConfig([credentialsId: 'kubelogin']) {
		  sh ('kubectl create namespace argocd')
		        sh ('kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml')
		           sh ('kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'')
		}
	      }
   	}
	
 }
}
