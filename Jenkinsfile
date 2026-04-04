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
                sh '''
                    if [ -d prometheus.yml ]; then
                        rm -rf prometheus.yml
                    fi
                    if [ ! -f prometheus.yml ]; then
                        cat > prometheus.yml << 'EOF'
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
EOF
                    fi
                '''
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