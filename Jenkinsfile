pipeline {
    agent any

    environment {
        NODE_OPTIONS = "--openssl-legacy-provider"
    }

    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/betawins/Trading-UI.git'
            }
        }

        stage('Install & Build') {
            steps {
                sh '''
                    echo "Cleaning up old files..."
                    rm -rf node_modules package-lock.json build .env

                    echo "Disabling preflight check to fix eslint conflict..."
                    echo SKIP_PREFLIGHT_CHECK=true > .env

                    echo "Installing dependencies..."
                    npm install

                    echo "Running build..."
                    npm run build
                '''
            }
        }

        stage('Start with PM2') {
            steps {
                sh '''
                    echo "Starting app with PM2..."
                    pm2 delete Trading-UI || true
                    pm2 --name Trading-UI start npm -- start
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        failure {
            echo '❌ Build failed!'
        }
        success {
            echo '✅ Build succeeded and app started!'
        }
    }
}
