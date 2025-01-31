pipeline {
    agent any

    environment {
        IMAGE_NAME = "node-app"
        CONTAINER_NAME = "node-container"
        NETWORK_NAME = "app-network"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/LyudmilMihaylov/-NodeJS'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                }
            }
        }

        stage('Stop and Remove Old Container') {
            steps {
                script {
                    sh 'docker rm -f ${CONTAINER_NAME} || true'
                }
            }
        }

        stage('Create Docker Network (if needed)') {
            steps {
                script {
                    sh 'docker network create ${NETWORK_NAME} || true'
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    sh 'docker run -d -p 3000:3000 --name ${CONTAINER_NAME} --network ${NETWORK_NAME} ${IMAGE_NAME}:latest'
                }
            }
        }

        stage('Post Actions') {
            steps {
                echo "Deployment successful! App running at http://<EC2_PUBLIC_IP>:3000"
            }
        }
    }
}
