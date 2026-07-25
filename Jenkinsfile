pipeline {
    agent any

    environment {
        IMAGE_NAME = "projet-devsecops"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Test Application') {
            steps {
                sh 'docker run -d -p 5001:5000 --name test-container ${IMAGE_NAME}:${IMAGE_TAG}'
                sh 'sleep 10'
                sh 'docker logs test-container'
                sh 'curl -f http://localhost:5001/health || exit 1'
                sh 'docker stop test-container'
                sh 'docker rm test-container'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'echo "Scan de securite a venir (Trivy)"'
            }
        }
    }

    post {
        always {
            sh 'docker rm -f test-container || true'
        }
        success {
            echo 'Pipeline reussi !'
        }
        failure {
            echo 'Pipeline echoue.'
        }
    }
}
