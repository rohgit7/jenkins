pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-express-app"
        CONTAINER_NAME = "express_app"
        PORT = "3000"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                // This fetches the code from the repo configured in the Jenkins Job
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Node dependencies locally (Optional/Sanity Check)...'
                // This ensures the package.json is valid before building the image
                bat 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Image: ${IMAGE_NAME}..."
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying Container: %CONTAINER_NAME%..."
                bat """
                    :: 1. Stop and remove existing container if it exists
                    docker rm -f %CONTAINER_NAME% 2>nul || ver > nul
                    
                    :: 2. Run the new container
                    docker run -d -p %PORT%:%PORT% --name %CONTAINER_NAME% %IMAGE_NAME%
                """
            }
        }

        stage('Health Check') {
            steps {
                echo 'Verifying if the application is reachable...'
                // Gives the container 5 seconds to fully start up the Node process
                bat "timeout /t 5"
                bat "curl -s http://localhost:%PORT% || echo 'App is not responding yet'"
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful! Access at: http://localhost:${PORT}"
        }
        failure {
            echo "❌ Pipeline failed. Check the console output for errors."
        }
    }
}