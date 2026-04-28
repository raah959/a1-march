pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:24.0.7
    image: docker:27.0.3
    command:
    - cat
    tty: true
    volumeMounts:
    - name: docker-sock
      mountPath: /var/run/docker.sock
  - name: helm
    image: alpine/helm:3.14.0
    command:
    - cat
      tty:true
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
"""
        }
    }

    environment {
        DOCKER_IMAGE = "mrdevops0959/devops-demo:v2"
    }

    stages {

        stage('Build Image') {
            steps {
                container('docker') {
                    sh 'docker build -t $DOCKER_IMAGE app/'
                }
            }
        }

      stage('Push Image') {
          steps {
              container('docker') {
                  withCredentials([usernamePassword(
                      credentialsId: 'dockerhub-creds',
                      usernameVariable: 'DOCKER_USER',
                      passwordVariable: 'DOCKER_PASS'
                  )]) {
                      sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push $DOCKER_IMAGE
                      '''
                  }
              }
          }
      }

        stage('Deploy') {
            steps {
                container('helm') {
                sh 'helm upgrade devops-demo ./devops-demo'
                }
            }
        }
    }
}
