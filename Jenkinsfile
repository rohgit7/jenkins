pipeline {
    agent any

    environment {
        PORT = '3000'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/rohgit7/jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                bat '''
                docker build -t my-express-app .
                '''
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo 'Deploying container...'
                bat '''
                docker rm -f express_app || exit /b 0
                docker run -d -p 3000:3000 --name express_app my-express-app
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Verifying deployment...'
                bat '''
                docker ps | findstr express_app
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
