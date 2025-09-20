pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ananthikaav/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Redirect output to test.log, always succeed
                bat 'npm test > test.log 2>&1 || exit 0'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'test.log', fingerprint: true
                }
            }
        }

        stage('Coverage Report') {
            steps {
                // If coverage script exists
                bat 'npm run coverage > coverage.log 2>&1 || exit 0'
                archiveArtifacts artifacts: 'coverage/**', allowEmptyArchive: true
            }
        }

        stage('NPM Security Audit') {
            steps {
                bat 'npm audit --json > npm-audit.json || exit 0'
                archiveArtifacts artifacts: 'npm-audit.json', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo "Pipeline finished. Check artifacts and console output."
        }
    }
}
