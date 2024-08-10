pipeline {
    agent any

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
                    sh "docker tag todo-list-app ${env.dockerHubUser}/vireshkumar327/cisco:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/vireshkumar327/cisco:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

