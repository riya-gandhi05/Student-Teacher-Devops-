pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/riya-gandhi05/Student-Teacher-Devops-.git'
            }
        }

        stage('Clean Docker') {
            steps {
                sh 'docker-compose down -v --remove-orphans || true'
            }
        }

        stage('Fix Prometheus') {
            steps {
                sh '[ -d prometheus.yml ] && rm -rf prometheus.yml && git checkout -- prometheus.yml || echo "prometheus.yml is already a file, OK"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t student-app .'
            }
        }

        stage('Run Docker Compose') {
            steps {
                sh 'docker-compose up -d --build'
            }
        }
    }
}