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
                input message: 'Lanjutkan ke tahap Deploy? Pilih "Proceed" atau "Abort"',
                ok: 'Proceed',
                parameters: [choice(name: 'PILIHAN', choices: 'Proceed\nAbort', description: '')]
            }
        }
        stage('Deploy') {
            steps {
                sh  './jenkins/scripts/deliver.sh'
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
                script {
                    sleep(time: 60, unit: 'SECONDS')
                }
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}