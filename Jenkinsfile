pipeline {
    agent any

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
