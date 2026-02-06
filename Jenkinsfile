pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'ikomajic'
        IMAGE_NAME = 'project2-demo'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_USER}/${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        docker.image("${DOCKERHUB_USER}/${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }
    }
}
