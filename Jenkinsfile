pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/riya-gandhi05/Student-Teacher-Devops-.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t student-app .'
            }
        }

        stage('Run Docker Compose') {
            steps {
                bat 'docker-compose up -d'
            }
        }
    }
}