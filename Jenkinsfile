pipeline {
    agent any

    stages {
        stage("Verify Docker Installation") {
            steps {
                echo "Verifying Docker installation"
                sh "/opt/homebrew/bin/docker --version"
                sh "/opt/homebrew/bin/docker info"
            }
        }

        stage("Build") {
            steps {
                echo "Building the Docker image"
                sh "/opt/homebrew/bin/docker build -t vireshkumar327/cisco ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the Docker image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "/opt/homebrew/bin/docker tag todo-list-app ${env.dockerHubUser}/vireshkumar327/cisco:latest"
                    sh "/opt/homebrew/bin/docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "/opt/homebrew/bin/docker push ${env.dockerHubUser}/vireshkumar327/cisco:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh "/opt/homebrew/bin/docker-compose down && /opt/homebrew/bin/docker-compose up -d"
            }
        }
    }
}
