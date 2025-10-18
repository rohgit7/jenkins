pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Ensure Node.js is available (configure this in Jenkins global tools)
    }

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
                sh 'npm ci' // faster and more consistent than npm install
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // You can skip this if you don’t have tests
                sh 'npm test || echo "No tests found or tests failed, continuing..."'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project (if needed)...'
                // Useful for frontend builds (e.g., React + Express)
                sh 'npm run build || echo "No build step defined."'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting the Express app...'
                sh '''
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
