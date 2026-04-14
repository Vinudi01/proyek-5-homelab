pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('SAST: Semgrep') {
            steps {
                script {
                    // Kita gunakan direktori kerja saat ini
                    sh "docker run --rm -v /var/lib/docker/volumes/jenkins_home/_data/workspace/${JOB_NAME}:/src returntocorp/semgrep semgrep scan --config=p/php --config=p/security-audit --error"
                }
            }
        }
        stage('SCA: OSV-Scanner') {
            steps {
                sh 'docker run --rm -v "${WORKSPACE}:/src" ghcr.io/google/osv-scanner /src --recursive'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker-compose up -d --build'
            }
        }
    }
}
