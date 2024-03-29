pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan)'
                }
            }
        }
        stage('Deploy') {
            steps {
                timeout(time: 60, unit: 'SECONDS') {
                sh './jenkins/scripts/deliver.sh'
                    for (int i = 1; i <= 60; i++) {
                        echo "Detik ke-${i}"
                        sh 'sleep 1'
                    }
                }
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}