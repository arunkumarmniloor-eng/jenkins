pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "arundevops0047/node-ci-pipeline"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:%IMAGE_TAG% ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push %DOCKER_IMAGE%:%IMAGE_TAG%"
            }
        }

        stage('Deploy Container') {
            steps {
                bat """
                docker stop node-ci-container || exit 0
                docker rm node-ci-container || exit 0
                docker run -d -p 3000:3000 --name node-ci-container %DOCKER_IMAGE%:%IMAGE_TAG%
                """
            }
        }
    }
}