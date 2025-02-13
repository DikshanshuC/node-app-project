
// pipeline {
//     agent any
//     environment {
//         AWS_ACCOUNT_ID = '557690623737'
//         AWS_REGION = 'ap-south-1'
//         FRONTEND_REPO_NAME = 'frontend-repo'
//         BACKEND_REPO_NAME = 'node-app-repo'
//         MYSQL_REPO_NAME = 'mysql-repo'
//         FRONTEND_IMAGE_NAME = 'frontend-app'
//         BACKEND_IMAGE_NAME = 'node-app'
//         MYSQL_IMAGE_NAME = 'mysql-db'
//     }
//     stages {
        
//         stage('Build Backend Docker Image') {
//             steps {
//                 sh 'docker build -t ${BACKEND_IMAGE_NAME} ./backend'
//             }
//         }
//         stage('Build Frontend Docker Image') {
//             steps {
//                 sh 'docker build -t ${FRONTEND_IMAGE_NAME} ./frontEnd'
//             }
//         }
//         stage('Build MySQL Docker Image') {
//             steps {
//                 sh 'docker build -t ${MYSQL_IMAGE_NAME} ./mysql'
//             }
//         }
//         stage('Login to AWS ECR') {
//             steps {
//                 sh 'aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com'
//             }
//         }
//         stage('Tag & Push Backend Image to ECR') {
//             steps {
//                 sh '''
//                 docker tag ${BACKEND_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest
//                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest
//                 '''
//             }
//         }
//         stage('Tag & Push Frontend Image to ECR') {
//             steps {
//                 sh '''
//                 docker tag ${FRONTEND_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest
//                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest
//                 '''
//             }
//         }
//         stage('Tag & Push MySQL Image to ECR') {
//             steps {
//                 sh '''
//                 docker tag ${MYSQL_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest
//                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest
//                 '''
//             }
//         }
//         stage('Deploy to Server') {
//             steps {
//                 sh '''
//                 ssh -o StrictHostKeyChecking=no ec2-user@your-server-ip "
//                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest &&
//                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest &&
//                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest &&
//                 docker stop ${BACKEND_REPO_NAME} || true &&
//                 docker rm ${BACKEND_REPO_NAME} || true &&
//                 docker run -d -p 3000:3000 --name ${BACKEND_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest &&
//                 docker stop ${FRONTEND_REPO_NAME} || true &&
//                 docker rm ${FRONTEND_REPO_NAME} || true &&
//                 docker run -d -p 80:80 --name ${FRONTEND_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest &&
//                 docker stop ${MYSQL_REPO_NAME} || true &&
//                 docker rm ${MYSQL_REPO_NAME} || true &&
//                 docker run -d -p 3306:3306 --name ${MYSQL_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest"
//                 '''
//             }
//         }
//     }
// }


// pipeline {
//     agent any

//     environment {
//         AWS_REGION = 'ap-south-1'
//         AWS_ACCOUNT_ID = '557690623737'
//         FRONTEND_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/frontend-repo'
//         BACKEND_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/node-app-repo'
//         MYSQL_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/mysql-repo'
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git 'https://github.com/DikshanshuC/node-app-project.git'
//             }
//         }

//         stage('Login to AWS ECR') {
//             steps {
//                 withCredentials([aws(credentialsId: 'aws-credentials', region: 'ap-south-1')]) {
//                     sh '''
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $FRONTEND_REPO
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $BACKEND_REPO
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $MYSQL_REPO
//                     '''
//                 }
//             }
//         }

//         stage('Build Docker Images') {
//             steps {
//                 sh '''
//                 docker build -t $FRONTEND_REPO:latest ./frontend
//                 docker build -t $BACKEND_REPO:latest ./backend
//                 docker build -t $MYSQL_REPO:latest ./mysql
//                 '''
//             }
//         }

//         stage('Push Docker Images to ECR') {
//             steps {
//                 sh '''
//                 docker push $FRONTEND_REPO:latest
//                 docker push $BACKEND_REPO:latest
//                 docker push $MYSQL_REPO:latest
//                 '''
//             }
//         }

//         stage('Deploy Containers') {
//             steps {
//                 sh '''
//                 docker-compose -f docker-compose.yml down
//                 docker-compose -f docker-compose.yml up -d
//                 '''
//             }
//         }
//     }
// }



// pipeline {
//     agent any

//     environment {
//         AWS_REGION = 'ap-south-1'
//         AWS_ACCOUNT_ID = '557690623737'
//         FRONTEND_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/frontend-repo'
//         BACKEND_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/node-app-repo'
//         MYSQL_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/mysql-repo'
//         AWS_ACCESS_KEY_ID = 'AKIAYDWHTS346SICR754'
//         AWS_SECRET_ACCESS_KEY = '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'
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git 'https://github.com/DikshanshuC/node-app-project.git'
//             }
//         }

//         stage('Login to AWS ECR') {
//             steps {
//                 // withEnv(["AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID", "AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY"]) {
//                     sh '''
//                     aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 557690623737.dkr.ecr.ap-south-1.amazonaws.com
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $FRONTEND_REPO
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $BACKEND_REPO
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $MYSQL_REPO
//                     '''
//                 // }
//             }
//         }
//             stage('Build Backend Docker Image') {
// //             steps {
// //                 sh 'docker build -t ${BACKEND_IMAGE_NAME} ./backend'
// //             }
// //         }
// //         stage('Build Frontend Docker Image') {
// //             steps {
// //                 sh 'docker build -t ${FRONTEND_IMAGE_NAME} ./frontEnd'
// //             }
// //         }
// //         stage('Build MySQL Docker Image') {
// //             steps {
// //                 sh 'docker build -t ${MYSQL_IMAGE_NAME} ./mysql'
// //             }
// //         }
//     }
//             stage('Tag & Push Backend Image to ECR') {
//             steps {
// //                 sh '''
// //                 docker tag ${BACKEND_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest
// //                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest
// //                 '''
// //             }
// //         }
// //         stage('Tag & Push Frontend Image to ECR') {
// //             steps {
// //                 sh '''
// //                 docker tag ${FRONTEND_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest
// //                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest
// //                 '''
// //             }
// //         }
// //         stage('Tag & Push MySQL Image to ECR') {
// //             steps {
// //                 sh '''
// //                 docker tag ${MYSQL_IMAGE_NAME}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest
// //                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest
// //                 '''
// //             }
// //         }
// //         stage('Deploy to Server') {
// //             steps {
// //                 sh '''
// //                 ssh -o StrictHostKeyChecking=no ec2-user@your-server-ip "
// //                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest &&
// //                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest &&
// //                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest &&
// //                 docker stop ${BACKEND_REPO_NAME} || true &&
// //                 docker rm ${BACKEND_REPO_NAME} || true &&
// //                 docker run -d -p 3000:3000 --name ${BACKEND_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO_NAME}:latest &&
// //                 docker stop ${FRONTEND_REPO_NAME} || true &&
// //                 docker rm ${FRONTEND_REPO_NAME} || true &&
// //                 docker run -d -p 80:80 --name ${FRONTEND_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO_NAME}:latest &&
// //                 docker stop ${MYSQL_REPO_NAME} || true &&
// // //                 docker rm ${MYSQL_REPO_NAME} || true &&
// // //                 docker run -d -p 3306:3306 --name ${MYSQL_REPO_NAME} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO_NAME}:latest"
// // //                 '''
// // //             }
// // //         }
// // //     }
            
// // }

 
 
pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '557690623737'
        FRONTEND_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/frontend-repo'
        BACKEND_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/backend-repo'
        MYSQL_REPO = '557690623737.dkr.ecr.ap-south-1.amazonaws.com/mysql-repo'
        AWS_ACCESS_KEY_ID = 'AKIAYDWHTS346SICR754'
        AWS_SECRET_ACCESS_KEY = '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'
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

        stage('Build Backend Docker Image') {
            steps {
                sh 'docker build -t ${BACKEND_REPO}:latest ./backend'
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                sh 'docker build -t ${FRONTEND_REPO}:latest ./frontend'
            }
        }

        stage('Build MySQL Docker Image') {
            steps {
                sh 'docker build -t ${MYSQL_REPO}:latest'
            }
        }

        stage('Tag & Push Backend Image to ECR') {
            steps {
                sh '''
                docker tag ${BACKEND_REPO}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest
                
                   // docker tag backend-image:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest
                   // docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest
                    '''
                    
            }
        }

        stage('Tag & Push Frontend Image to ECR') {
            steps {
                sh '''
                docker tag ${FRONTEND_REPO}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest
                '''
            }
        }

        stage('Tag & Push MySQL Image to ECR') {
            steps {
                sh '''
                docker tag ${MYSQL_REPO}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest
                '''
            }
        }

        stage('Deploy to Server') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ec2-user@your-server-ip "
                docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest &&
                docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest &&
                docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest &&
                docker stop ${BACKEND_REPO} || true &&
                docker rm ${BACKEND_REPO} || true &&
                docker run -d -p 3000:3000 --name ${BACKEND_REPO} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest &&
                // docker stop backend-container || true
                // docker rm backend-container || true
                // docker run -d -p 3000:3000 --name backend-container ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest

                docker stop ${FRONTEND_REPO} || true &&
                docker rm ${FRONTEND_REPO} || true &&
                docker run -d -p 8000:8000 --name ${FRONTEND_REPO} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest &&
                docker stop ${MYSQL_REPO} || true &&
                docker rm ${MYSQL_REPO} || true &&
                docker run -d -p 3306:3306 --name ${MYSQL_REPO} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest"
                // docker stop frontend-container || true
                // docker rm frontend-container || true
                // docker run -d -p 8000:8000 --name frontend-container ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest

                // docker stop mysql-container || true
                // docker rm mysql-container || true
                // docker run -d -p 3306:3306 --name mysql-container -e MYSQL_ROOT_PASSWORD=root ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest

                 '''
            }
        }
    }
}



// end of finalcode



// pipeline {
//     agent any

//     environment {
//         AWS_REGION = 'ap-south-1'
//         AWS_ACCOUNT_ID = '557690623737'
//         FRONTEND_REPO = 'frontend-repo'
//         BACKEND_REPO = 'node-app-repo'
//         MYSQL_REPO = 'mysql-repo'
//         AWS_ACCESS_KEY_ID = 'AKIAYDWHTS346SICR754'
//         AWS_SECRET_ACCESS_KEY = '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'
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git 'https://github.com/DikshanshuC/node-app-project.git'
//             }
//         }

//         stage('Login to AWS ECR') {
//             steps {
//                 sh '''
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/$FRONTEND_REPO
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/$BACKEND_REPO
//                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/$MYSQL_REPO
//                 '''
//             }
//         }

//         stage('Build Backend Docker Image') {
//             steps {
//                 sh 'docker build -t ${BACKEND_REPO}:latest ./backend'
//             }
//         }

//         stage('Build Frontend Docker Image') {
//             steps {
//                 sh 'docker build -t ${FRONTEND_REPO}:latest ./frontend'
//             }
//         }

//         stage('Build MySQL Docker Image') {
//             steps {
//                 sh 'docker build -t ${MYSQL_REPO}:latest ./mysql'
//             }
//         }

//         stage('Tag & Push Backend Image to ECR') {
//             steps {
//                 sh '''
//                 docker tag ${BACKEND_REPO}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest
//                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest
//                 '''
//             }
//         }

//         stage('Tag & Push Frontend Image to ECR') {
//             steps {
//                 sh '''
//                 docker tag ${FRONTEND_REPO}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest
//                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest
//                 '''
//             }
//         }

//         stage('Tag & Push MySQL Image to ECR') {
//             steps {
//                 sh '''
//                 docker tag ${MYSQL_REPO}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest
//                 docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest
//                 '''
//             }
//         }

//         stage('Deploy to Server') {
//             steps {
//                 sh '''
//                 ssh -o StrictHostKeyChecking=no ec2-user@your-server-ip "
//                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest &&
//                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest &&
//                 docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest &&
//                 docker stop ${BACKEND_REPO} || true &&
//                 docker rm ${BACKEND_REPO} || true &&
//                 docker run -d -p 3000:3000 --name ${BACKEND_REPO} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${BACKEND_REPO}:latest &&
//                 docker stop ${FRONTEND_REPO} || true &&
//                 docker rm ${FRONTEND_REPO} || true &&
//                 docker run -d -p 8000:8000 --name ${FRONTEND_REPO} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${FRONTEND_REPO}:latest &&
//                 docker stop ${MYSQL_REPO} || true &&
//                 docker rm ${MYSQL_REPO} || true &&
//                 docker run -d -p 3306:3306 --name ${MYSQL_REPO} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${MYSQL_REPO}:latest"
//                 '''
//             }
//         }
//     }
// }
