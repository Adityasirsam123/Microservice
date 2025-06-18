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
                // This checks out branch "cartservice" directly
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/Adityasirsam123/Microservice.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Dockerfile is in the repo root of this branch
                    sh "docker build -t ${IMAGE_NAME} ."
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
