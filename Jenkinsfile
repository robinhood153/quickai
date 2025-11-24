pipeline {
    agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
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
                dir('client') {
                    script {
                        docker.build("quickai-client:${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Build Server') {
            steps {
                dir('server') {
                    script {
                        docker.build("quickai-server:${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}
