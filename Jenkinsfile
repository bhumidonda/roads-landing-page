pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'dondabhumi/page'
    }
    
    stages {
        stage("Code Clone") {
            steps {
                echo "Cloning repository..."
                git url: "https://github.com/bhumidonda/node.git", branch: "main"
            }
        }

        stage("Build Docker Image") {
            steps {
                echo "Building Docker image..."
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage("Push To DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds", 
                    usernameVariable: "dockerHubUser", 
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                    sh "docker image tag $DOCKER_IMAGE:latest $dockerHubUser/$DOCKER_IMAGE:latest"
                    sh "docker push $dockerHubUser/$DOCKER_IMAGE:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying application..."
                sh "docker compose down || true"
                sh "docker compose up -d --build"
            }
        }
    }
}
