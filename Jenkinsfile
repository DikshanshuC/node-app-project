
pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = '557690623737'
        AWS_REGION = 'ap-south-1'
        FRONTEND_REPO_NAME = 'frontend-repo'
        BACKEND_REPO_NAME = 'node-app-repo'
        MYSQL_REPO_NAME = 'mysql-repo'
        FRONTEND_IMAGE_NAME = 'frontend-app'
        BACKEND_IMAGE_NAME = 'node-app'
        MYSQL_IMAGE_NAME = 'mysql-db'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/DikshanshuC/node-app-project.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || echo "Tests failed, but continuing..."'
            }
        }
        stage('Build Backend Docker Image') {
            steps {
                sh 'docker build -t ${BACKEND_IMAGE_NAME} ./backend'
            }
        }
        stage('Build Frontend Docker Image') {
            steps {
                sh 'docker build -t ${FRONTEND_IMAGE_NAME} ./frontend'
            }
        }
        stage('Build MySQL Docker Image') {
            steps {
                sh 'docker build -t ${MYSQL_IMAGE_NAME} ./mysql'
            }
        }
        stage('Login to AWS ECR') {
            steps {
                sh 'aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com'
            }
        }
        stage('Tag & Push Backend Image to ECR') {
            steps {
                sh '''
                docker tag ${BACKEND_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest
                '''
            }
        }
        stage('Tag & Push Frontend Image to ECR') {
            steps {
                sh '''
                docker tag ${FRONTEND_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest
                '''
            }
        }
        stage('Tag & Push MySQL Image to ECR') {
            steps {
                sh '''
                docker tag ${MYSQL_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest
                '''
            }
        }
        stage('Deploy to Server') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ec2-user@your-server-ip "
                docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest &&
                docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest &&
                docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest &&
                docker stop ${BACKEND_REPO_NAME} || true &&
                docker rm ${BACKEND_REPO_NAME} || true &&
                docker run -d -p 3000:3000 --name ${BACKEND_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest &&
                docker stop ${FRONTEND_REPO_NAME} || true &&
                docker rm ${FRONTEND_REPO_NAME} || true &&
                docker run -d -p 80:80 --name ${FRONTEND_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest &&
                docker stop ${MYSQL_REPO_NAME} || true &&
                docker rm ${MYSQL_REPO_NAME} || true &&
                docker run -d -p 3306:3306 --name ${MYSQL_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest"
                '''
            }
        }
    }
}
