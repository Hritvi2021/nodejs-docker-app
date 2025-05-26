pipeline {
    agent any

    environment {
        IMAGE_NAME = 'yourdockerhubusername/nodejs-webapp'
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds' // We'll create this in Step 6
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo-name.git'
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
                    docker.withRegistry('', "${DOCKER_CREDENTIALS_ID}") {
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

