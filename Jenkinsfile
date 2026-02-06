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
                bat "docker build -t %DOCKERHUB_USER%/%IMAGE_NAME%:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push %DOCKERHUB_USER%/%IMAGE_NAME%:latest"
            }
        }

        stage('Deploy to Kubernetes with Helm') {
            environment {
                KUBECONFIG = 'C:\\Users\\ivanm\\.kube\\config'
            }    
            steps {
                bat """
                echo ===== CURRENT DIRECTORY =====
                echo ===== CURRENT DIRECTORY =====
                cd
                echo ===== LIST ROOT =====
                dir
                echo ===== LIST HELM FOLDER =====
                dir helm
                echo ===== HELM TEMPLATE DEBUG =====
                helm template project2 helm --debug
                echo ===== HELM INSTALL =====
                helm upgrade --install project2 helm --debug
                """
            }
        }
    }
}
