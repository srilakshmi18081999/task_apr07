pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_id') // Jenkins stored credentials
        IMAGE_NAME = 'srilakshmi1808/web_host'  // Update with your Docker Hub repo
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'master', url: 'https://github.com/srilakshmi18081999/task_apr07.git' 
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    bat "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    bat "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Run Nginx Container in Background') {
            steps {
                script {
                    bat "docker run -d --name nginx-server -p 80:80 ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo "Nginx container is running on port 80!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}