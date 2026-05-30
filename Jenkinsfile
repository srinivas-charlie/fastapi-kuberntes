pipeline {

    agent any

    environment {
        IMAGE_NAME = "localhost:5000/fastapi-app"
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('Push') {
            steps {
                sh """
                docker push ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        stage('Deploy') {
            steps {
                sh """
                kubectl set image deployment/fastapi \
                fastapi=${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        stage('Rollout Status') {
            steps {
                sh """
                kubectl rollout status deployment/fastapi
                """
            }
        }
    }
}