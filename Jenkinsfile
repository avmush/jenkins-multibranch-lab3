pipeline {
    agent any

    environment {
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
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
                // Add your build commands here, e.g., npm install
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests..."
                // Add your test commands here, e.g., npm test
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "myapp:${env.BRANCH_NAME}"
                    echo "🐳 Building Docker image: ${imageTag}"
                    sh "docker build -t ${imageTag} ."
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying app on port ${PORT}"
                // Example run (optional): sh "docker run -d -p ${PORT}:${PORT} myapp:${env.BRANCH_NAME}"
            }
        }
    }
}

