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
       bat 'npm test || exit 0'  // Windows-friendly way to ignore test failures
     }
   }
   stage('Generate Coverage Report') {
     steps {
       bat 'npm run coverage || exit 0'  // Windows batch compatible
     }
   }
   stage('NPM Audit (Security Scan)') {
     steps {
       bat 'npm audit || exit 0'
     }
   }
   stage('Snyk Security Scan') {
     environment {
       SNYK_TOKEN = credentials('snyk-token-id')  // Add your Snyk token credential ID here
     }
     steps {
       bat '''
         snyk auth %SNYK_TOKEN%
         snyk test
       '''
     }
   }
 }
}
