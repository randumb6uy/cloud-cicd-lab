pipeline {
    agent any
    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REGISTRY = '313951301619.dkr.ecr.ap-south-1.amazonaws.com'
        IMAGE_NAME = 'image-regi'
        ECR_IMAGE = "${ECR_REGISTRY}/${IMAGE_NAME}:latest"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build and Test') {
            steps {
                sh 'python3 -m pip install --user -r requirements.txt'
                sh 'python3 -m py_compile app.py'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }
        stage('ECR Login') {
            steps {
                sh 'aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}'
            }
        }
        stage('Docker Tag') {
            steps {
                sh 'docker tag ${IMAGE_NAME}:latest ${ECR_IMAGE}'
            }
        }
        stage('Push to ECR') {
            steps {
                sh 'docker push ${ECR_IMAGE}'
            }
        }
    }
}
