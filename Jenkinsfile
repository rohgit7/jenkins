pipeline {
    agent any

    environment {
        PORT = '3000'
        WORKSPACE_DIR = '/var/jenkins_home/workspace/marvelPipeline'
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
                sh """
                    cd $WORKSPACE_DIR
                    docker build -t my-express-app .
                """
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo 'Deploying Express app container...'
                sh """
                    # Remove existing container if any
                    docker rm -f express_app || true

                    # Run new container with port mapping
                    docker run -d -p 3000:3000 --name express_app my-express-app
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking if Express app is running...'
                sh """
                    docker ps | grep express_app
                    curl -s http://localhost:3000 || echo "App not responding"
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
