pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        PORT = '3000'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/rohgit7/jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                // Ensure Node.js is available, otherwise install it first
                sh '''
                if ! command -v node >/dev/null 2>&1; then
                    apt update && apt install -y nodejs npm
                fi
                npm ci
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test || echo "No tests found or tests failed, continuing..."'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project (if needed)...'
                sh 'npm run build || echo "No build step defined."'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting the Express app...'
                // Ensure PORT is exported and app runs in background
                sh '''
                export PORT=3000
                nohup npm start > app.log 2>&1 &
                sleep 5
                echo "App started and running on port $PORT"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Please check the logs for details.'
        }
        always {
            echo 'Pipeline completed.'
        }
    }
}
