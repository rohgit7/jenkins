pipeline {
    agent any

    environment {
        PORT = '3000'
        // Use double backslashes for Windows paths in Groovy strings
        WORKSPACE_DIR = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\marvelPipeline"
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
                echo 'Building Docker image for Express app...'
                // Use 'bat' for Windows Command Prompt
                bat """
                    cd /d "%WORKSPACE_DIR%"
                    docker build -t my-express-app .
                """
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo 'Deploying Express app container...'
                bat """
                    :: Remove existing container if any (ignore error if it doesn't exist)
                    docker rm -f express_app || ver > nul

                    :: Run new container with port mapping
                    docker run -d -p 3000:3000 --name express_app my-express-app
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking if Express app is running...'
                bat """
                    docker ps | findstr express_app
                    :: Note: curl is available in modern Windows (Cmd/PowerShell)
                    curl -s http://localhost:3000 || echo App not responding
                """
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully! Express app is live at http://localhost:3000'
        }
        failure {
            echo '❌ Pipeline failed. Check logs for details.'
        }
        always {
            echo 'Pipeline completed.'
        }
    }
}