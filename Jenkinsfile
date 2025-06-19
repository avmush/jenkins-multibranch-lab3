pipeline {
    agent any

    environment {
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "avmush/myapp:${env.BRANCH_NAME}"
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

        stage('Dockerfile Lint (Hadolint)') {
            steps {
                echo "🔍 Linting Dockerfile with Hadolint..."
                // Don't fail build on lint error
                sh 'hadolint Dockerfile || true'
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

        stage('Trivy Scan') {
            steps {
                script {
                    echo "🛡️ Running Trivy vulnerability scan..."
                    sh "trivy image --severity HIGH,CRITICAL --no-progress --exit-code 0 ${IMAGE_NAME}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        echo "📦 Logging in and pushing Docker image..."
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying app on port ${PORT}"
                // Example: run container
                // sh "docker run -d -p ${PORT}:${PORT} ${IMAGE_NAME}"
            }
        }
    }
}

