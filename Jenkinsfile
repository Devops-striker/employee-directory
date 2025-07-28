pipeline {
    agent { label 'workernode' }

    tools {
        maven 'maven' // 👈 This name must match Jenkins Global Tool config
    }

    environment {
        TOMCAT_USER = 'ubuntu'
        TOMCAT_HOST = '13.235.133.33'
        TOMCAT_PATH = '/opt/tomcat/webapps'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Cloning code from Git'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '🔧 Building application...'
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo '🚀 Deploying WAR to Tomcat server...'
                sh '''
                    scp -o StrictHostKeyChecking=no target/*.war ${TOMCAT_USER}@${TOMCAT_HOST}:${TOMCAT_PATH}/
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed. Check logs.'
        }
    }
}
