pipeline {
    agent any

    environment {
        IMAGE_NAME = 'sneha730/hello_django'   // Correct Docker Hub repo name (your Docker Hub username)
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds' // Jenkins credentials ID
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Sneha-S-U/hello_django.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${IMAGE_NAME}").push("latest")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh """
                        # Stop and remove any existing container
                        docker stop hello_django || true
                        docker rm hello_django || true
                        
                        # Pull the latest image from Docker Hub
                        docker pull ${IMAGE_NAME}:latest
                        
                        # Run the new container on port 80 (public-facing) mapped to port 8000 (Django default port)
                        docker run -d -p 80:8000 --name hello_django ${IMAGE_NAME}:latest
                    """
                }
            }
        }
    }
}
