pipeline {
    agent any

    environment {
        APP_NAME = 'Lab-2026-SpringBoot'
        DEPLOY_DIR = '/opt/apps'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'Github_credentials',
                    url: 'git@github.com:satishlabs/Lab-2026-SpringBoot.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                sh """
                  mkdir -p ${DEPLOY_DIR}
                  pkill -f ${APP_NAME}.jar || true
                  cp target/*.jar ${DEPLOY_DIR}/${APP_NAME}.jar
                  nohup java -jar ${DEPLOY_DIR}/${APP_NAME}.jar > ${DEPLOY_DIR}/app.log 2>&1 &
                """
            }
        }

        stage('Verify') {
            steps {
                sh "curl -f http://localhost:8080/hello"
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful'
        }
        failure {
            echo '❌ Deployment Failed'
        }
    }
}
