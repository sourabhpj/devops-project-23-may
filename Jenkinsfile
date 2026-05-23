pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'sourabhpj94'
        IMAGE_NAME      = 'my-nginx-image'
        TAG             = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t sourabhpj94/my-nginx-image:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh 'docker push sourabhpj94/my-nginx-image:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop my-nginx-container || true'
                sh 'docker rm my-nginx-container || true'
                sh 'docker run -d --name my-nginx-container -p 80:80 sourabhpj94/my-nginx-image:latest'
            }
        }
    }
}