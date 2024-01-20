pipeline {
    agent { labels "dev-server"}
    stages {
        stage("build"){
           steps {
                git url: "https://github.com/ashis90/node-todo-cicd.git", branch: "master"
                echo "code is cloned"
            }
        }
        stage("build and test"){
            steps {
                sh "docker build -t node-app ."
                echo "code is built and tested"
            }
        }
         stage("scan"){
            steps {
                echo "image is scanned"
            }
        }
        stage("image pushed to dockerhub"){
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker tag node-app:latest ${env.dockerhubUser}/node-app:latest"
                sh "docker push ${env.dockerhubUser}/node-app:latest"
                echo "image has been pushed to dockerhub"
                }
            }
        }
        stage("deployed"){
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo "deployed in production"
            }
        }
    }
}
