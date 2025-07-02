pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/yourusername/my-ci-cd-project.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${DOCKER_IMAGE}:latest", '.')
        }

      }
    }

    stage('Push to DockerHub') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
            docker.image("${DOCKER_IMAGE}:latest").push()
          }
        }

      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s-deployment.yaml'
      }
    }

  }
  environment {
    DOCKER_IMAGE = 'yourdockerhubusername/my-app'
    DOCKER_CREDENTIALS_ID = 'dockerhub-creds'
  }
}