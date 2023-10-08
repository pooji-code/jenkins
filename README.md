pipeline {
    agent any

    environment {
        // Define environment variables as needed
        DOCKERFILE_PATH = './Dockerfile'  // Path to your Dockerfile
        DOCKER_IMAGE_NAME = 'poojiofc/docker-image'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out your source code from your version control system (e.g., Git)
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image using the Dockerfile
                script {
                    def dockerImage = docker.build("${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}", "--file ${env.DOCKERFILE_PATH} .")
                    dockerImage.push()
                }
            }
        }
    }

    post {
        success {
            // Actions to take on successful image build and push (e.g., notifications)
            echo 'Docker image build and push were successful!'
        }
        failure {
            // Actions to take on image build or push failure (e.g., notifications, rollback)
            echo 'Docker image build or push failed!'
        }
    }
}
