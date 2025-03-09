pipeline {
    agent any


    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo "Checking out code from branch: main"
                    checkout scm
                }
            }
        }

        stage('Setup Environment') {
            steps {
                sh 'sudo dnf install -y java-17-openjdk maven'
                sh 'sudo dnf install maven -y'
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    echo "Running tests for ${params.SERVICE}"
                    sh "./mvnw test"
                }
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                    jacoco execPattern: '**/target/jacoco.exec'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "./mvnw clean install"
            }
        }
    }

    post {
        success { echo "Integrate successfully" }
        failure { echo "Integrate failed!" }
    }
}
