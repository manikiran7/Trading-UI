pipeline {
    agent any

    environment {
        APP_NAME = "Trading-UI"
        WORK_DIR = "${env.WORKSPACE}/build"
        NODE_OPTIONS = "--openssl-legacy-provider"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/betawins/Trading-UI.git'
            }
        }

        stage('Install & Build') {
            steps {
                sh 'npm install'
                sh 'npm audit fix || true' // allow non-fatal audit
                sh 'npm run build'
            }
        }

        stage('Start with PM2') {
            steps {
                sh """
                    pm2 delete ${APP_NAME} || true
                    cd ${WORK_DIR}
                    pm2 --name ${APP_NAME} start npm -- start
                """
            }
        }

        stage('Post Build Cleanup') {
            steps {
                sh 'rm -rf node_modules'
                sh 'npm cache clean --force'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
