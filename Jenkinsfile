pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mrdevops0959/devops-demo"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/raah959/a1-march.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:v2 app/'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:v2'
            }
        }

        stage('Deploy with Helm') {
            steps {
                sh 'helm upgrade devops-demo ./devops-demo'
            }
        }
    }
}
