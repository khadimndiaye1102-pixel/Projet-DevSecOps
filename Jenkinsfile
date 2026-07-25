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
                sh 'docker run -d --name test-container ${IMAGE_NAME}:${IMAGE_TAG}'
                sh 'sleep 10'
                sh 'docker logs test-container'
                sh '''
                    CONTAINER_IP=$(docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" test-container)
                    echo "IP du conteneur : $CONTAINER_IP"
                    curl -f http://$CONTAINER_IP:5000/health
                '''
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
