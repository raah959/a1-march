pipeline {
    agent {
        docker {
            image 'docker:24.0.7'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        DOCKER_IMAGE = "mrdevops0959/devops-demo:v2"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE app/'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy with Helm') {
            steps {
                sh 'helm upgrade devops-demo ./devops-demo'
            }
        }
    }
}
