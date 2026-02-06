pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'ikomajic'
        IMAGE_NAME = 'project2-demo'
        K8S_RELEASE = 'project2-helm'
        HELM_CHART_PATH = 'helm/project2-helm'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
            }
        }

        stage('Deploy to Kubernetes with Helm') {
            steps {
                sh '''
                echo "Checking Kubernetes access"
                kubectl get nodes

                echo "Deploying application with Helm"
                helm upgrade --install project2-helm helm/project2-helm
                '''
            }
        }
    }
}
