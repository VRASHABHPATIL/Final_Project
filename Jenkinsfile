pipeline {
    agent { label 'agent1' }

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REGISTRY = '203918845423.dkr.ecr.ap-south-1.amazonaws.com'
        FRONTEND_IMAGE = "${ECR_REGISTRY}/finalproject/frontend:latest"
        BACKEND_IMAGE  = "${ECR_REGISTRY}/finalproject/server:latest"
    }

    stages {
        
        stage('Checkout Terraform Code') {
            steps {
                git credentialsId: 'github',
                    url: 'https://github.com/VRASHABHPATIL/Final_Project.git',
                    branch: 'main'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin $ECR_REGISTRY
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh '''
                    docker build -t frontend-image ./frontend
                    docker tag frontend-image:latest $FRONTEND_IMAGE
                '''
            }
        }

        stage('Build Backend Image') {
            steps {
                sh '''
                    docker build -t backend-image ./server
                    docker tag backend-image:latest $BACKEND_IMAGE
                '''
            }
        }

        stage('Push Images to ECR') {
            steps {
                sh '''
                    docker push $FRONTEND_IMAGE
                    docker push $BACKEND_IMAGE
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logout $ECR_REGISTRY'
        }
    }
}

