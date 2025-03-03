@Library("jenkins-shared-library") _
pipeline {
    agent { label "dev" }

    stages {

        stage('welcome') {
            steps {
                welcome()
            }
        }

        // stage("code") {
        //     steps {
        //         git url: "https://github.com/safi-siddiqui-github/two-tier-flask-app.git", branch: "main"
        //     }
        // }

        // stage("scan") {
        //     steps {
        //         sh "trivy fs ."
        //     }
        // }
        
        // stage("build & push") {
        //     steps {
        //         withCredentials([usernamePassword(
        //             credentialsId: "docker_hub",
        //             passwordVariable: "password",
        //             usernameVariable: "username",
        //         )]){
        //             sh "docker login -u ${env.username} -p ${env.password}"
        //             sh "docker build -t ${env.username}/ttfa_img:latest ."
        //             sh "docker push ${env.username}/ttfa_img:latest"
        //         }
        //     }
        // }
        
        // stage("deploy") {
        //     steps {
        //         sh "docker compose up -d --build app"
        //     }
        // }

        // stage("clean") {
        //     steps {
        //         sh "docker system prune -f"
        //     }
        // }

        // stage("down") {
        //     steps {
        //         sh "docker compose down"
        //     }
        // }
        
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