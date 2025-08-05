pipeline {
    agent any

    stages {
        stage('Deploy Backend to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/backend/'
            }
        }

        stage('Deploy Frontend to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/frontend/'
            }
        }

        stage('Deploy MySQL to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/mysql/'
            }
        }

        stage('Apply Ingress Controller') {
            steps {
                sh 'kubectl apply -f k8s/ingress/'
            }
        }
    }
}

