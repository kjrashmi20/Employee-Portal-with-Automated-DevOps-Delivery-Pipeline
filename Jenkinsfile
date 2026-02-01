pipeline {
    agent any
environment {
        IMAGE_NAME = "kjrashmi20/ecom-web"
        IMAGE_TAG  = "latest"
    }
stages {
stage('Checkout Code') {
            steps {
                git credentialsId: 'github-creds',
                    url: 'https://github.com/kjrashmi20/Employee-Portal-with-Automated-DevOps-Delivery-Pipeline.git'
            }
        }
stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }
stage('Docker Login') {
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
stage('Push Docker Image') {
            steps {
                sh '''
                docker push $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
post {
        success {
            echo "Docker image pushed successfully"
        }
    }
}
