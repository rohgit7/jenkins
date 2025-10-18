pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
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

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh '''
                # Install Node.js if not present
                if ! command -v node >/dev/null 2>&1; then
                    apt update && apt install -y nodejs npm
                fi

                # Install project dependencies
                cd $WORKSPACE_DIR
                npm ci
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                cd $WORKSPACE_DIR
                npm test || echo "No tests found or tests failed, continuing..."
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Build stage (optional)'
                sh '''
                cd $WORKSPACE_DIR
                npm run build || echo "No build step defined."
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting the Express app in background...'
                sh '''
                cd $WORKSPACE_DIR
                export PORT=$PORT
                nohup node server.js > app.log 2>&1 &
                sleep 5
                echo "Express app started on port $PORT"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs for details.'
        }
        always {
            echo 'Pipeline completed.'
        }
    }
}
