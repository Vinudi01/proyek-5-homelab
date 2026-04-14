pipeline {
    agent any

    stages {
        // --- STAGE 1: Tarik Kode ---
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // --- STAGE 2: SCAN KEAMANAN (Taruh di Sini!) ---
        stage('SAST - Semgrep (Fast Scan)') {
            steps {
                script {
                    // Pakai tanda kutip dua (") biar variabel ${WORKSPACE} terbaca
                    sh "docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v ${WORKSPACE}:/src returntocorp/semgrep semgrep --config=auto --error"
                }
            }
        }

        // --- STAGE 3: ANALISIS LANJUTAN (SonarQube) ---
        stage('SAST - SonarQube (Deep Analysis)') {
            steps {
                echo "Lanjut ke Sonar..."
                // ... isi script sonarmu ...
            }
        }

        // --- STAGE 4: DEPLOY ---
        stage('Build & Deploy') {
            steps {
                sh "docker compose up -d"
            }
        }
    }
}