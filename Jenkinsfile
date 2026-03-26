pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '''
                echo "===== BUILD STARTED ====="
                date
                ls -l
                cat index.html
                echo "===== BUILD COMPLETED ====="
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                echo "===== DOCKER BUILD STARTED ====="
                docker build -t ci-app:latest .
                docker images
                echo "===== DOCKER BUILD COMPLETED ====="
                '''
            }
        }
    }
}
