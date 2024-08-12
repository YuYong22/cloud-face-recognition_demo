pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS_ID = 'Dockerhub'
        RECIPIENTS = 'phong3baox@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YuYong22/cloud-face-recognition_demo.git'
            }
        }
        stage('Build and Push API Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_HUB_CREDENTIALS_ID) {
                        def apiImage = docker.build("yuyong22/face-recognition-api:latest", "./api")
                        apiImage.push()
                    }
                }
            }
        }
        stage('Build and Push DB Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_HUB_CREDENTIALS_ID) {
                        def dbImage = docker.build("yuyong22/face-recognition-db:latest", "./database")
                        dbImage.push()
                    }
                }
            }
        }
    }

    post {
        success {
            emailext (
                subject: "Jenkins Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <p>Tin tốt đây!</p>
                    <p>Build cho job <b>${env.JOB_NAME}</b> số build <b>${env.BUILD_NUMBER}</b> đã thành công.</p>
                    <p>Kiểm tra chi tiết <a href="${env.BUILD_URL}">tại đây</a>.</p>
                """,
                to: env.RECIPIENTS,
                mimeType: 'text/html'
            )
        }
        failure {
            emailext (
                subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <p>Rất tiếc, build cho job <b>${env.JOB_NAME}</b> số build <b>${env.BUILD_NUMBER}</b> đã thất bại.</p>
                    <p>Kiểm tra chi tiết <a href="${env.BUILD_URL}">tại đây</a>.</p>
                """,
                to: env.RECIPIENTS,
                mimeType: 'text/html'
            )
        }
    }
}
