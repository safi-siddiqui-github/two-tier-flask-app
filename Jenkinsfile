pipeline {
    agent { label "dev" }

    stages {

        stage("code") {
            steps {
                git url: "https://github.com/safi-siddiqui-github/two-tier-flask-app.git", branch: "main"
            }
        }
        
        stage("build") {
            steps {
                sh "docker pull python:3-slim"
                sh "docker build -t ttfa_img ."
                sh "docker pull mysql:latest"
            }
        }
        
        
        stage("push") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "docker_hub",
                    passwordVariable: "password",
                    usernameVariable: "username",
                )]){
                    sh "docker login -u ${env.username} -p ${env.password}"
                    sh "docker image tag ttfa_img ${env.username}/ttfa_img"
                    sh "docker push ${env.username}/ttfa_img:latest"
                }
            }
        }
        
        stage("deploy") {
            steps {
                sh "docker compose up -d --build app"
            }
        }
    }
}