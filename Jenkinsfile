pipeline {

    agent any

    environment {
        IMAGE_NAME = "mydocker3692/spring-petclinic"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/yesk993-ops/spring-petclinic.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh '''
                chmod +x mvnw
                ./mvnw clean package -DskipTests
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Docker Run (Local Test)') {
            steps {
                sh '''
                docker rm -f petclinic || true
                docker run -d -p 8080:8080 --name petclinic $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/
                '''
            }
        }

    }

}
