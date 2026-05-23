pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('build') {
            steps {
                sh 'docker build -t my-nginx-image .'
            }
        }
        stage('deploy') {
            steps {
                sh 'docker stop my-nginx-container || true'
                sh 'docker rm my-nginx-container || true'
                sh 'docker run -d --name my-nginx-container -p 80:80 my-nginx-image'
                
            }
        }
    }
}