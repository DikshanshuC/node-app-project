pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '248189928204'
        FRONTEND_REPO = '248189928204.dkr.ecr.ap-south-1.amazonaws.com/frontend-repo'
        BACKEND_REPO = '248189928204.dkr.ecr.ap-south-1.amazonaws.com/backend-repo'
        MYSQL_REPO = '248189928204.dkr.ecr.ap-south-1.amazonaws.com/mysql-repo'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/DikshanshuC/node-app-project.git'
            }
        }

        stage('Login to AWS ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $FRONTEND_REPO
                    aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $BACKEND_REPO
                    aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $MYSQL_REPO
                '''
            }
        }

        stage('Build Docker Images') {
            parallel {
                stage('Backend Image') {
                    steps {
                        sh 'docker build -t ${BACKEND_REPO}:latest ./backend'
                    }
                }
                stage('Frontend Image') {
                    steps {
                        sh 'docker build -t ${FRONTEND_REPO}:latest ./frontEnd'
                    }
                }
                stage('MySQL Image') {
                    steps {
                        sh 'docker build -t ${MYSQL_REPO}:latest ./mysql'
                    }
                }
            }
        }

        stage('Push Docker Images to ECR') {
            parallel {
                stage('Push Backend Image') {
                    steps {
                        sh 'docker push ${BACKEND_REPO}:latest'
                    }
                }
                stage('Push Frontend Image') {
                    steps {
                        sh 'docker push ${FRONTEND_REPO}:latest'
                    }
                }
                stage('Push MySQL Image') {
                    steps {
                        sh 'docker push ${MYSQL_REPO}:latest'
                    }
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                sh '''
                aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com &&
                
                docker pull ${BACKEND_REPO}:latest &&
                docker pull ${FRONTEND_REPO}:latest &&
                docker pull ${MYSQL_REPO}:latest &&
                
                docker stop backend || true &&
                docker rm backend || true &&
                docker run -d -p 8000:8000 --name backend --restart unless-stopped ${BACKEND_REPO}:latest &&
                
                docker stop frontend || true &&
                docker rm frontend || true &&
                docker run -d -p 5000:5000 --name frontend --restart unless-stopped ${FRONTEND_REPO}:latest &&
                
                docker stop mysql || true &&
                docker rm mysql || true &&
                docker run -d -p 3306:3306 --name mysql --restart unless-stopped ${MYSQL_REPO}:latest
                '''
            }
        }
    }
}
