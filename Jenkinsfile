pipeline {

    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/yesk993-ops/spring-petclinic.git'
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

        stage('Build Image via Docker Compose') {
            steps {
                sh '''
                docker compose build
                '''
            }
        }

        stage('Run Containers (Local Test)') {   // ✅ FIXED HERE
            steps {
                sh '''
                docker compose down || true
                docker compose up -d
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/
                '''
            }
        }

    }

}
