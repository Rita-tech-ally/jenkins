@Library('common-utils@main') _

pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/iforimran9/secretsanta-generator.git'
            }
        }

        stage('Pre Build Checks') {
            steps {
                preBuildChecks()
            }
        }

        stage('GitLeaks Scan') {
            steps {
                gitLeaksScan()
            }
        }

        stage('Sonar Analysis') {
            steps {
                sonarAnalysis()
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                owaspDependencyCheck()
            }
        }

        stage('Test') {
            steps {
                mavenTest()
            }
        }
    }

    post {

        success {
            echo "Build SUCCESS"

            emailext (
                to: "your-email@gmail.com,rituc7707@gmail.com",
                subject: "Build SUCCESS",
                body: "Pipeline completed successfully 👍"
            )

            emailNotification("SUCCESS")
        }

        failure {
            echo "Build FAILED"

            emailext (
                to: "your-email@gmail.com,rituc7707@gmail.com",
                subject: "Build FAILED",
                body: "Check Jenkins logs ❌"
            )

            emailNotification("FAILED")
        }

        always {
            echo "CI Pipeline Completed"
        }
    }
}
