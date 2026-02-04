pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "ikomajic/project2"
    IMAGE_TAG    = "${BUILD_NUMBER}"
    IMAGE        = "${DOCKER_IMAGE}:${IMAGE_TAG}"
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Docker Build') {
      steps {
        bat '''
          docker build -t %IMAGE% .
        '''
      }
    }

    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          bat '''
            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
          '''
        }
      }
    }

    stage('Push Image') {
      steps {
        bat '''
          docker push %IMAGE%
        '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        bat '''
          helm upgrade --install project2 ./helm ^
            --set image.repository=%DOCKER_IMAGE% ^
            --set image.tag=%IMAGE_TAG%
        '''
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline completed successfully'
    }
    failure {
      echo '❌ Pipeline failed'
    }
  }
}
