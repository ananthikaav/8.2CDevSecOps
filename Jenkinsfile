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
                bat 'npm ci'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests, capture logs
                bat 'npm test 2>&1 | tee test.log || true'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'test.log', fingerprint: true
                }
            }
        }

        stage('Coverage Report') {
            steps {
                bat 'npm run coverage 2>&1 | tee coverage.log || true'
                archiveArtifacts artifacts: 'coverage/**', allowEmptyArchive: true
            }
        }

        stage('NPM Security Audit') {
            steps {
                // Run npm audit, save results
                bat 'npm audit --json > npm-audit.json || true'
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
