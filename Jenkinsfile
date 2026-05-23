pipeline {
    agent any

    environment {
        DOCKER_HUB_USER  = 'sourabhbhpj94'
        IMAGE_NAME       = 'my-nginx-image'
        TAG              = 'latest'
        EKS_CLUSTER_NAME = 'my-eks-cluster'
        AWS_REGION       = 'ap-south-1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // सिंगल कोट्समुळे व्हेरिएबल्स अगदी परफेक्ट रीड होतील
                sh 'docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${TAG} .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // क्रेडेंशियल आयडी आपण नवीन बनवलेला 'docker-hub-new-token' वापरला आहे
                withCredentials([usernamePassword(credentialsId: 'docker-hub-new-token', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin'
                    sh 'docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${TAG}'
                }
            }
        }

        stage('Deploy to EKS Cluster') {
            steps {
                // kubeconfig आधीच अपडेट झाला असल्यामुळे थेट डिप्लॉयमेंट कमांड चालेल
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}