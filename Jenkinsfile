pipeline {
    agent { label "dev" }
    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/iamamash/React-Django-Demo-App.git", branch: "main"
            }
        }
        stage("Build and Test") {
            steps {
                sh "docker build . -t react-app"
            }
        }
        stage("Login and Push") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"password",usernameVariable:"user")]) {
                    sh "docker login -u ${env.user} -p ${password}"
                    sh "docker tag react-app ${env.user}/react_django_demo_app:latest"
                    sh "docker push ${env.user}/react_django_demo_app:latest"
                }
            }
        }
        stage("Deployed") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
