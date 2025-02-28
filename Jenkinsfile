pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'dondabhumi/page'
    }

    stages {
        stage("Clone Repository") {
            steps {
                echo "Cloning repository..."
                git url: 'https://github.com/bhumidonda/node.git', branch: 'main'
            }
        }

        stage("Build Docker Image") {
            steps {
                echo "Building Docker image..."
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "DOCKER_CREDENTIALS_ID", 
                    usernameVariable: "DOCKER_USER", 
                    passwordVariable: "DOCKER_PASS"
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker tag $DOCKER_IMAGE:latest $DOCKER_USER/$DOCKER_IMAGE:latest"
                    sh "docker push $DOCKER_USER/$DOCKER_IMAGE:latest"
                }
            }
        }

        stage("Deploy Container") {
            steps {
                echo "Stopping existing container..."
                sh "docker stop nextjs-app || true"
                sh "docker rm nextjs-app || true"
                echo "Running new container..."
                sh "docker run -d -p 3000:3000 --name nextjs-app $DOCKER_IMAGE:latest"
            }
        }
    }
}
