pipeline {
    agent any

    environment {
        ECR_REGISTRY = '566252492534.dkr.ecr.eu-north-1.amazonaws.com'
        IMAGE_NAME = 'devops-demo'
        IMAGE_TAG = '1.0'
        AWS_REGION = 'eu-north-1'
    }

    stages {

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $ECR_REGISTRY/$IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('ECR Login') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin $ECR_REGISTRY
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'docker push $ECR_REGISTRY/$IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}
