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
                sh "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${TAG} ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-new-token', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${TAG}"
                }
            }
        }

        stage('Deploy to EKS Cluster') {
            steps {
                script {
                    // Jenkins ला क्लस्टरचा लेटेस्ट ॲक्सेस मिळवून देणे
                    sh "aws eks update-kubeconfig --region ${AWS_REGION} --name ${EKS_CLUSTER_NAME}"
                    
                    // Kubernetes वर पॉड्स आणि लोड बॅलन्सर अप्लाय करणे
                    sh "kubectl apply -f deployment.yaml"
                    
                    // Zero Downtime साठी रोलआउट रीस्टार्ट करणे
                    sh "kubectl rollout restart deployment/nginx-deployment"
                }
            }
        }
    }
}