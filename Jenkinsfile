pipeline {
    agent any

    environment {
        IMAGE_NAME = "aadityasirsam/${env.BRANCH_NAME}:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/Adityasirsam123/Microservice.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh "echo \$PASS | docker login -u \$USER --password-stdin"
                        sh "docker push ${IMAGE_NAME}"
                    }
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
