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
            emailext (
                subject: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """\
                    <p><strong>Build Status:</strong> ${currentBuild.currentResult}</p>
                    <p><strong>Project:</strong> ${env.JOB_NAME}</p>
                    <p><strong>Build Number:</strong> ${env.BUILD_NUMBER}</p>
                    <p><strong>Console Output:</strong> <a href="${env.BUILD_URL}">View here</a></p>
                    <p>Artifacts attached: test log, coverage report, audit results.</p>
                """,
                mimeType: 'text/html',
                to: 'ananthikaa.vivek@gmail.com',
                attachmentsPattern: 'test.log, coverage.log, npm-audit.json'
            )
            echo "Email notification sent."
        }
    }
}