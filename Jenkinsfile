pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'textgrab_ocr-ocr_app-1'
    }

    stages {
        stage("Login to DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "DOCKER_CREDENTIALS_ID", 
                    usernameVariable: "DOCKER_USER", 
                    passwordVariable: "DOCKER_PASS"
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage("Pull and Deploy Container") {
            steps {
                sh "docker pull $DOCKER_IMAGE:latest"
                sh "docker stop nextjs-app || true"
                sh "docker rm nextjs-app || true"
                sh "docker run -d -p 8800:8800 --name nextjs-app $DOCKER_IMAGE:latest"
            }
        }
    }
}
