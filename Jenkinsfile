pipeline {
    agent any

    environment {
        DOCKER_IMAGE_API = 'yuyong22/face-recognition-api'
        DOCKER_IMAGE_DB = 'yuyong22/face-recognition-db'
        REGISTRY_CREDENTIALS = 'Dockerhub'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'Github', url: 'https://github.com/YuYong22/cloud-face-recognition_demo.git'
            }
        }

        stage('Build API Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_API}:latest", './api/Dockerfile')
                }
            }
        }

        stage('Build Database Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_DB}:latest", './database/Dockerfile')
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', "${REGISTRY_CREDENTIALS}") {
                        docker.image("${DOCKER_IMAGE_API}:latest").push()
                        docker.image("${DOCKER_IMAGE_DB}:latest").push()
                    }
                }
            }
        }
    }

    post {
        success {
            emailext (
                to: 'phong3baox@gmail.com',
                subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The build was successful.\n\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nURL: ${env.BUILD_URL}"
            )
        }
        failure {
            emailext (
                to: 'phong3baox@gmail.com',
                subject: "Build Failure: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The build failed.\n\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nURL: ${env.BUILD_URL}"
            )
        }
    }
}
