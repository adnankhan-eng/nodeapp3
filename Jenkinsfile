pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/your-username/nodeapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t your-dockerhub-username/nodeapp .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker login -u your-username -p your-password'
                sh 'docker push your-dockerhub-username/nodeapp'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}