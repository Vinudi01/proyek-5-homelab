pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('SAST: Semgrep') {
            steps {
                // Gunakan single quotes untuk sh agar ${WORKSPACE} tidak dievaluasi oleh shell lokal
                sh 'docker run --rm -v "${WORKSPACE}:/src" returntocorp/semgrep semgrep scan --config="p/php" --config="p/security-audit" --error'
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
