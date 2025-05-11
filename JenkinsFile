pipeline {
    agent any

    environment {
        IMAGE_NAME = 'snehasu/hello_django'   // Docker Hub repo name
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds' // Jenkins credentials ID
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Sneha-S-U/hello_django.git'
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
                    docker stop hello_django || true
                    docker rm hello_django || true
                    docker pull ${IMAGE_NAME}:latest
                    docker run -d -p 80:8000 --name hello_django ${IMAGE_NAME}:latest
                    """
                }
            }
        }
    }
}
