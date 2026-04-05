pipeline {
    agent any

    stages {

        stage('Build Image') {
            steps {
                script {
                    if (isUnix()) {
                        sh """
                        docker build -t student-app:${BUILD_NUMBER} .
                        docker tag student-app:${BUILD_NUMBER} student-app:latest
                        """
                    } else {
                        bat """
                        docker build -t student-app:%BUILD_NUMBER% .
                        docker tag student-app:%BUILD_NUMBER% student-app:latest
                        """
                    }
                }
            }
        }

        stage('Deploy (FAST)') {
            steps {
                script {
                    if (isUnix()) {
                        sh """
                        docker stop fsdbproject-app || true
                        docker rm fsdbproject-app || true
                        docker-compose -p fsdbproject up -d
                        """
                    } else {
                        bat """
                        docker stop fsdbproject-app || exit 0
                        docker rm fsdbproject-app || exit 0
                        docker-compose -p fsdbproject up -d
                        """
                    }
                }
            }
        }

        stage('Reload Prometheus') {
            steps {
                script {
                    if (isUnix()) {
                        sh "docker exec fsdbproject-prometheus-1 kill -HUP 1 || true"
                    } else {
                        bat "docker exec fsdbproject-prometheus-1 kill -HUP 1 || exit 0"
                    }
                }
            }
        }
    }
}
