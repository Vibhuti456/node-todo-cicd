pipeline {
    agent { label "dev-server"}
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/Vibhuti456/node-todo-cicd.git", branch: "master"
                echo 'code has cloned in slave system'
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo 'image has built in slave system'
            }
        }
        stage("scan image"){
            steps{
                echo 'image has been scanned'
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'Image has been pushed in slave system'
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker compose up -d"
                echo 'Deployement has been done successfully'
            }
        }
    }
}
