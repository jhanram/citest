pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ci-app .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    docker tag ci-app $USER/ci-app:latest
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $USER/ci-app:latest
                    echo "===== DOCKER PUSH COMPLETED ====="
                    '''
                }
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sh '''
                cd ansible
                ansible-playbook -i inventory playbook.yml
                '''
            }
        }
    }
