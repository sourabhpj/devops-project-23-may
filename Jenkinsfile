pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'sourabhpj94'
        IMAGE_NAME = 'my-nginx-image'
        TAG = 'Latest'
    }

    stages {
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('build') {
            steps {
                sh 'docker build -t sourabhpj94/my-nginx-image:Latest .'
            }
        }
        stage('push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')])
                sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                sh 'docker push sourabhpj94/my-nginx-image:Latest'
                }
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