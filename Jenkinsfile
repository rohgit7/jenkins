pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rohgit7/jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                echo 'Build stage (optional for frontend apps)'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting the Express app...'
                sh 'npm start &'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed!'
        }
    }
}
