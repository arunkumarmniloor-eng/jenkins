pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                echo 'Cloning Repository...'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t node-ci-demo .'
            }
        }

        stage('List Docker Images') {
            steps {
                bat 'docker images'
            }
        }
    }
}