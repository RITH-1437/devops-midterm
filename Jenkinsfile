pipeline {
    agent { label 'spring' }

    environment {
        JAVA_HOME = '/opt/jdk-25'
        PATH = "/opt/jdk-25/bin:/opt/maven/bin:${env.PATH}"
        PROJECT_NAME = 'devops-midterm-RITH'
        DEPLOY_URL = 'http://178.128.93.188/Midterm-2026/Rin-Nairith'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RITH-1437/devops-midterm.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    export JAVA_HOME=/opt/jdk-25
                    export PATH=$JAVA_HOME/bin:$PATH
                    chmod +x mvnw
                    ./mvnw clean package -DskipTests
                '''
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh '''
                    ansible-playbook ansible/deploy.yml \
                        -i ansible/inventory.ini \
                        --extra-vars "jar_path=$(pwd)/target/demo-0.0.1-SNAPSHOT.jar index_path=$(pwd)/src/main/resources/templates/index.html build_number=${BUILD_NUMBER}"
                '''
            }
        }
    }

    post {
        success {
            mail(
                to: 'nairithrin143@gmail.com',
                subject: "${env.PROJECT_NAME} - Build #${env.BUILD_NUMBER} SUCCESS",
                body: """
Build SUCCESS!

Project     : ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL   : ${env.BUILD_URL}

Your app has been deployed at:
${env.DEPLOY_URL}
                """
            )
        }
        failure {
            mail(
                to: 'nairithrin143@gmail.com',
                subject: "${env.PROJECT_NAME} - Build #${env.BUILD_NUMBER} FAILED",
                body: """
Build FAILED!

Project     : ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL   : ${env.BUILD_URL}

Please check the console output for the full error stacktrace.
                """
            )
        }
    }
}
