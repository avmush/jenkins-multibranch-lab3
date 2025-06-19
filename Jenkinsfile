pipeline {
    agent any

    environment {
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "myapp:${env.BRANCH_NAME}"
        DOCKERHUB_USER = "avmush"
        DOCKERHUB_IMAGE = "${DOCKERHUB_USER}/myapp:${env.BRANCH_NAME}"
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
                // Add real build commands if needed
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests..."
                // Add real test commands if needed
            }
        }

        stage('Dockerfile Lint (Hadolint)') {
            steps {
                echo "🔍 Linting Dockerfile with Hadolint..."
                sh 'hadolint Dockerfile'
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
                echo "🔎 Scanning image with Trivy..."
                sh "trivy image --severity HIGH,CRITICAL --no-progress --exit-code 0 ${IMAGE_NAME}"
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker tag ${IMAGE_NAME} ${DOCKERHUB_IMAGE}
                        docker push ${DOCKERHUB_IMAGE}
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying app on port ${PORT}"
                // Optional: sh "docker run -d -p ${PORT}:${PORT} ${DOCKERHUB_IMAGE}"
            }
        }
    }
}

