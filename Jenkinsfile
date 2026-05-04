pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RITH-1437/devops-midterm.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }

    post {
        failure {
            mail(
                to: 'your@gmail.com',
                subject: "devops-midterm-RITH - Build #${env.BUILD_NUMBER} FAILED",
                body: """
Build FAILED!

Project : ${env.JOB_NAME}
Build Number : ${env.BUILD_NUMBER}
Build URL : ${env.BUILD_URL}

Please check the console output for the full error stacktrace.
                """
            )
        }
    }
}
