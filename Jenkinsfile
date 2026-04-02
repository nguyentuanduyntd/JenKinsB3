pipeline {
    agent any

    options {
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install & Lint & Build') {
            agent {
                docker {
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                dir('my-react-app') {
                    sh 'node -v && npm -v'
                    sh 'npm install'
                    sh 'npm run lint'
                    sh 'npm run build'
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'my-react-app/dist/**/*', fingerprint: true, allowEmptyArchive: false
        }
        failure {
            echo 'Pipeline thất bại'
        }
    }
}