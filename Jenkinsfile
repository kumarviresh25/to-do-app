pipeline {
    agent any

    environment {
        PATH = "/opt/homebrew/bin:$PATH"
    }

    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/kumarviresh25/to-do-app.git", branch: "main"
            }
        }

        stage("Verify Docker Installation") {
            steps {
                echo "Verifying Docker installation"
                sh "docker --version"
                sh "docker info"
            }
        }

        stage("Build") {
            steps {
                echo "Building the Docker image"
                sh "docker build -t vireshkumar327/cisco ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the Docker image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "docker tag vireshkumar327/cisco ${env.dockerHubUser}/cisco:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/cisco:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Stopping and starting Docker containers"
                sh "docker-compose stop && docker-compose up -d"
            }
        }
    }
}
