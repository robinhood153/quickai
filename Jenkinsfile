pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: dind
    image: docker:dind
    securityContext:
      privileged: true
    env:
      - name: DOCKER_TLS_CERTDIR
        value: ""
    tty: true
"""
        }
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Client') {
            steps {
                container('dind') {
                    dir('client') {
                        sh '''
                            echo "Building CLIENT Docker Image..."
                            docker build -t quickai-client:${BUILD_ID} .
                            docker image ls
                        '''
                    }
                }
            }
        }

        stage('Build Server') {
            steps {
                container('dind') {
                    dir('server') {
                        sh '''
                            echo "Building SERVER Docker Image..."
                            docker build -t quickai-server:${BUILD_ID} .
                            docker image ls
                        '''
                    }
                }
            }
        }
    }
}
