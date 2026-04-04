pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/riya-gandhi05/Student-Teacher-Devops-.git'
            }
        }

    stage('Clean Docker') {
        sh '''
        docker-compose down -v --remove-orphans
        docker system prune -f
        docker volume prune -f
        '''
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

        stage('Configure Prometheus') {
            steps {
                sh '''
                    docker exec student-teacher-devops-prometheus-1 sh -c "cat > /etc/prometheus/prometheus.yml << EOF
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets:
          - localhost:9090
  - job_name: student-app
    static_configs:
      - targets:
          - app:4000
EOF"
                    docker exec student-teacher-devops-prometheus-1 kill -HUP 1 || true
                '''
            }
        }
    }
}