pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:$PATH"
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
                sh "/opt/homebrew/bin/docker --version"
                sh "/opt/homebrew/bin/docker info"
            }
        }

        stage("Build") {
            steps {
                echo "Building the Docker image"
                sh "/opt/homebrew/bin/docker build -t vireshkumar327/to-do-app ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the Docker image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "/opt/homebrew/bin/docker tag vireshkumar327/to-do-app ${env.dockerHubUser}/to-do-app:latest"
                    sh "/opt/homebrew/bin/docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "/opt/homebrew/bin/docker push ${env.dockerHubUser}/to-do-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Stopping and starting Docker containers"
                sh "/usr/local/bin/docker-compose stop && /usr/local/bin/docker-compose up -d"
            }
        }
    }
}
