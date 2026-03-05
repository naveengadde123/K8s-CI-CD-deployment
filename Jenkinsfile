pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "dockerhubusername/flask-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/naveengadde123/K8s-CI-CD-deployment'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub', url: '']) {
                    sh 'docker push $DOCKER_IMAGE:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}