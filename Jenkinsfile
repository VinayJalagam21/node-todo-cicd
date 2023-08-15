pipeline {
    agent { label "dev" }
    stages{
        stage("clone the code"){
            steps{
                git url: "https://github.com/VinayJalagam21/node-todo-cicd.git" , branch: "master"
            }
        }
        stage("Build and test"){
            steps{
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("docker push"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHub',passwordVariable: 'dockerHubPass',usernameVariable: 'dockerHubUser')]){
                sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d --build web"
            }
        }
    }
}
