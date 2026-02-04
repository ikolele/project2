pipeline {
  agent any

  environment {
    IMAGE = 'ikomajic/project2:latest'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Image') {
      steps {
        dir('app') {
          sh 'docker build -t $IMAGE .'
        }
      }
    }

    stage('Login to Docker Hub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
        }
      }
    }

    stage('Push Image') {
      steps {
        sh 'docker push $IMAGE'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'helm upgrade --install project2 ./helm'
      }
    }
  }
}
