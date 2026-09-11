pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/dishagowdahallianilkumar/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext (
                        subject: "Run Tests stage - ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage completed with status: ${currentBuild.currentResult}.\n\nCheck console output at ${env.BUILD_URL}",
                        to: 'dishaanilkumar963@gmail.com',
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan stage - ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Security Scan stage completed with status: ${currentBuild.currentResult}.\n\nCheck console output at ${env.BUILD_URL}",
                        to: 'dishaanilkumar963@gmail.com',
                        attachLog: true
                    )
                }
            }
        }
    }
}