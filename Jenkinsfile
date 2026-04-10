pipeline {
    agent any

    environment {
        // 🔹 Apna Docker Hub username yahan likho
        DOCKER_IMAGE = "adnankhan123/nodeapp"

        // 🔹 Har build ka unique tag (best practice)
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                // 🔹 Docker image build ho rahi hai
                sh 'docker build -t $DOCKER_IMAGE:$IMAGE_TAG .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                // 🔹 Jenkins credentials use ho rahe hain
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred' usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // 🔹 Docker Hub par push
                sh 'docker push $DOCKER_IMAGE:$IMAGE_TAG'

                // 🔹 Latest tag bhi update karo (optional but recommended)
                sh 'docker tag $DOCKER_IMAGE:$IMAGE_TAG $DOCKER_IMAGE:latest'
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

        stage('Run Container (Optional)') {
            steps {
                // 🔹 Old container remove (error ignore karega)
                sh 'docker rm -f nodeapp || true'

                // 🔹 New container run karo
                sh 'docker run -d -p 3000:3000 --name nodeapp $DOCKER_IMAGE:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // 🔹 Kubernetes deploy
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}