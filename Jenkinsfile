pipeline {

    agent any

    environment {
        IMAGE_NAME = "localhost:5000/fastapi-app"
       
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build DOCKER image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                """
            }
        }

        stage('Push TO local Registry') {
            steps {
                sh """
                docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                """
            }
        }

        stage('Deploy to KUBERNETES') {
            steps {
                sh """
                kubectl set image deployment/fastapi \
                fastapi=${IMAGE_NAME}:${BUILD_NUMBER}
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
        post {
        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Deployment Failed'
        }

    }
}