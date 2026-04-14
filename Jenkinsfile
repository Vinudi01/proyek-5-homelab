pipeline {
    agent any
    
    environment {
        SONAR_SCANNER_HOME = tool 'SonarScanner' // Nama ini harus sama dengan yang di-setup di Jenkins Global Tool
        DB_CREDS = credentials('wordpress-db-secrets')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SAST - Semgrep (Fast Scan)') {
            steps {
                script {
                    // Jalanin Semgrep via Docker buat ngecek celah keamanan dasar
                    sh 'docker run --rm -v $(pwd):/src returntocorp/semgrep semgrep --config=auto --error'
                }
            }
        }

        stage('SAST - SonarQube (Deep Analysis)') {
            steps {
                // Proses pengiriman kode ke SonarQube Dashboard (localhost:9000)
                withSonarQubeEnv('MySonarServer') { 
                    sh "${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=wordpress-homelab \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://sonarqube:9000"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Jenkins nunggu konfirmasi dari SonarQube: "Lulus atau Kagak?"
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build & Deploy') {
            steps {
                sh """
                    echo "DB_USER=${DB_USER}" > .env
                    echo "DB_PASS=${DB_PASS}" >> .env
                    docker compose up -d --build
                """
            }
        }
    }
}