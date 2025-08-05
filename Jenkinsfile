pipeline {
    agent any

    environment {
        K8S_NAMESPACE = "default"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '📥 Cloning repository...'
                checkout scm
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    echo '🐳 Building Laravel Docker image...'
                    sh 'docker build -t laravel-backend:$IMAGE_TAG .'
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    echo '🐳 Building React Docker image...'
                    sh 'docker build -t react-frontend:$IMAGE_TAG .'
                }
            }
        }

        stage('Deploy MySQL to Kubernetes') {
            steps {
                echo '🐘 Deploying MySQL...'
                sh 'kubectl apply -f k8s/mysql/'
            }
        }

        stage('Deploy Backend to Kubernetes') {
            steps {
                echo '🚀 Deploying Laravel backend...'
                sh 'kubectl apply -f k8s/backend/'
            }
        }

        stage('Deploy Frontend to Kubernetes') {
            steps {
                echo '🚀 Deploying React frontend...'
                sh 'kubectl apply -f k8s/frontend/'
            }
        }

        stage('Apply Ingress Rules') {
            steps {
                echo '🌐 Applying ingress configuration...'
                sh 'kubectl apply -f k8s/ingress/'
            }
        }

        stage('Restart Backend & Frontend Deployments') {
            steps {
                echo '🔁 Restarting deployments...'
                sh 'kubectl rollout restart deployment/laravel-backend'
                sh 'kubectl rollout restart deployment/react-frontend'
            }
        }

        stage('Wait for Rollout') {
            steps {
                echo '⏳ Waiting for pods to be ready...'
                sh 'kubectl rollout status deployment/laravel-backend'
                sh 'kubectl rollout status deployment/react-frontend'
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD pipeline completed successfully!'
            echo 'App should be available at: http://fullstack.local'
        }
        failure {
            echo '❌ CI/CD pipeline failed. Check the console logs.'
        }
    }
}
