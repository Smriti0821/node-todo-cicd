pipeline {
    agent any
    environment {
        appName = "node-app"
      	dockerHub = "Docker Creds"
    }
    stages { 
        stage("code"){
            steps{
                echo 'Cloning the Repository....'
                git url: "https://github.com/Smriti0821/node-todo-cicd.git",branch:"Test"
                echo 'Repository Cloned!!'
            }
        }
        stage("Building Images and list images"){
            steps{
                echo 'Building Image from Dockefile...'
                sh "sudo chmod 777 /var/run/docker.sock"
                sh "docker build . -t ${env.appName}"
                echo 'Image Build Completed!!'
                sh "docker images"
            }
        }
        stage("Pushing Image to Docker Hub"){
            steps{
                echo 'Pushing Image to Docker Hub'
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag ${env.appName}:latest ${env.dockerHubUser}/${env.appName}:latest"
                sh "docker push ${env.dockerHubUser}/${env.appName}:latest"
                echo 'Image pushed to Docker Hub'
                }
            }
        }
        stage("Deploy"){
            steps{
                echo 'Deployment Started....'
                sh "docker-compose down && docker-compose up -d"
                echo 'Deployment Completed!!!'
            }
        }
    }
}
