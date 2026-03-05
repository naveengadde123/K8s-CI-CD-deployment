pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app:latest .'
            }
        }

        stage('Load Image to Kind') {
            steps {
                sh 'kind load docker-image flask-app:latest --name dev-cluster'
            }
        }

        stage('Set Kubernetes Context') {
            steps {
                sh 'kubectl config use-context kind-dev-cluster'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc'
            }
        }
    }
}