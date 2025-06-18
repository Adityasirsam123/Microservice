pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-cred') // Jenkins credentials ID
        DOCKERHUB_USERNAME = "${DOCKER_HUB_CREDENTIALS_USR}"
        DOCKERHUB_PASSWORD = "${DOCKER_HUB_CREDENTIALS_PSW}"
        IMAGE_NAME = "aadityasirsam/${env.BRANCH_NAME}:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                // If cartservice is a branch and starts at "src/cartservice"
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/Adityasirsam123/Microservice.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // No need for dir() if Dockerfile is at repo root after checkout
                script {
                    sh "docker build -t ${IMAGE_NAME} -f src/cartservice/Dockerfile src/cartservice"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Successfully built and pushed ${IMAGE_NAME}"
        }
        failure {
            echo "❌ Failed to build/push Docker image for ${env.BRANCH_NAME}"
        }
    }
}
