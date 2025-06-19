pipeline {
    agent any

    environment {
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "myapp:${env.BRANCH_NAME}"
    }

    stages {
        stage('Set Port') {
            steps {
                echo "✅ Selected port: ${PORT}"
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
                echo "✅ Checked out branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Build') {
            steps {
                echo "🔧 Building the app for branch ${env.BRANCH_NAME} on port ${PORT}"
                // Example: sh 'npm install' or 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests..."
                // Example: sh 'npm test' or 'pytest tests/'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🐳 Building Docker image: ${IMAGE_NAME}"
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying app on port ${PORT}"
                // Optional run (example):
                // sh "docker run -d -p ${PORT}:${PORT} ${IMAGE_NAME}"
            }
        }
    }
}

