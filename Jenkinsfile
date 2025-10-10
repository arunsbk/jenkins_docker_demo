pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('Dockerhub_cred')   // ID we created
        IMAGE_NAME = "arunsbk/jenkins_docker_demo"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                // credentials() exposes variables DOCKERHUB_USR and DOCKERHUB_PSW (depending on Jenkins version, use .USR/.PSW or _USR/_PSW)
                sh 'echo "$DOCKERHUB_PSW" | docker login -u "$DOCKERHUB_USR" --password-stdin'
            }
        }

        stage('Push') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}

