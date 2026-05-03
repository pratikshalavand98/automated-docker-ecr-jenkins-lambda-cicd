pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ACCOUNT_ID = "922085930637"  
        REPO_NAME = "my-node-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/pratikshalavand98/automated-docker-ecr-jenkins-lambda-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $REPO_NAME .'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION \
                | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh '''
                docker tag $REPO_NAME:latest \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$REPO_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$REPO_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Image pushed to ECR successfully 🚀"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
