pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "saurab2h/devops-app"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDENTIALS_ID,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:v2 .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:v2'
            }
        }

        stage('Deploy to Kubernetes') {
    steps {
        sh '''
        export KUBECONFIG=/root/.kube/config
        kubectl get nodes
        kubectl apply -f deployment.yaml
        kubectl apply -f service.yaml
        '''
    }
}
    }
}
