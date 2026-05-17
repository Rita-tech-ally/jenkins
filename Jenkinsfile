pipeline {

    agent {
        label 'ubuntu-slave1'
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'master', description: 'Git Branch Name')

        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'test', 'prod'],
            description: 'Select Environment'
        )

        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run Unit Tests'
        )
    }

    tools {
        jdk 'JDK8'
    }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-8-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}",
                url: 'https://github.com/opstree/spring3hibernate.git'
            }
        }

        stage('GitLeaks Scan') {
            steps {
                sh '''
                    echo "Running GitLeaks Scan..."
                    gitleaks detect --source . || true
                '''
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Unit Test') {
            when {
                expression { params.RUN_TESTS == true }
            }
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Git Push') {
            steps {
                sh '''
                    git config user.name "jenkins"
                    git config user.email "jenkins@example.com"

                    git add .
                    git commit -m "Automated build commit" || true
                    git push origin ${BRANCH_NAME} || true
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed."
        }
    }
}
