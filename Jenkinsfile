pipeline {
    agent any

    environment {
        IMAGE_NAME = 'hritvi2021/nodejs-webapp'
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds' 
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Hritvi2021/nodejs-docker-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', "${dockerhub-creds}") {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker pull ${IMAGE_NAME}:latest
                docker stop nodejs-app || true
                docker rm nodejs-app || true
                docker run -d -p 3000:3000 --name nodejs-app ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}

