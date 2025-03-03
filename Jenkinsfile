pipeline {
    agent { label "dev" }

    stages {

        stage("install") {
            steps {
                sh "docker compose down"
                sh "sudo apt-get install wget gnupg"
                sh "wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null"
                sh "echo 'deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main' | sudo tee -a /etc/apt/sources.list.d/trivy.list"
                sh "sudo apt-get update"
                sh "sudo apt-get install trivy"
            }
        }

        stage("code") {
            steps {
                git url: "https://github.com/safi-siddiqui-github/two-tier-flask-app.git", branch: "main"
            }
        }
        
        stage("build & push") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "docker_hub",
                    passwordVariable: "password",
                    usernameVariable: "username",
                )]){
                    sh "docker login -u ${env.username} -p ${env.password}"
                    sh "docker build -t ${env.username}/ttfa_img:latest ."
                    sh "docker push ${env.username}/ttfa_img:latest"
                }
            }
        }
        
        stage("deploy") {
            steps {
                sh "docker compose up -d --build app"
            }
        }

        stage("clean") {
            steps {
                sh "docker system prune -f"
            }
        }
        
    }

    post {
        success {
            emailext from: "safi-siddiqui.com",
                subject: "Build Success",
                body: "Build Success: TTFA",
                to: "safisiddiqui.private@gmail.com"
        }

        failure {
            emailext from: "safi-siddiqui.com",
                subject: "Build Failed",
                body: "Build Failed: TTFA",
                to: "safisiddiqui.private@gmail.com"
        }
    }
}