pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: node
    image: node:20-alpine
    command:
    - cat
    tty: true
  - name: dind
    image: docker:dind
    securityContext:
      privileged: true
"""
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Debug Workspace') {
            steps {
                container('node') {
                    sh 'ls -laR'
                }
            }
        }

        stage('Install Server Dependencies') {
            steps {
                container('node') {
                    dir('server') {
                        sh 'npm install'
                    }
                }
            }
        }

        stage('Install Client Dependencies') {
            steps {
                container('node') {
                    dir('client') {
                        sh 'npm install'
                    }
                }
            }
        }

        stage('Build Server Image') {
            steps {
                container('dind') {
                    dir('server') {
                        sh 'docker build -t quickai-server:${BUILD_ID} .'
                    }
                }
            }
        }

        stage('Build Client Image') {
            steps {
                container('dind') {
                    dir('client') {
                        sh 'docker build -t quickai-client:${BUILD_ID} .'
                    }
                }
            }
        }
    }
}
