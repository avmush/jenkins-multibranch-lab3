pipeline {
    agent any

    environment {
        IMAGE_NAME = "nodeapp"
        PORT = (env.BRANCH_NAME == 'main') ? '3000' : '3001'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'npm test || true' // or your test command
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${env.BRANCH_NAME} ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // stop any existing container on that port
                    sh "docker ps -q --filter 'publish=${PORT}' | xargs -r docker stop || true"
                    // run app with port
                    sh "docker run -d -p ${PORT}:3000 --name ${env.BRANCH_NAME}_container ${IMAGE_NAME}:${env.BRANCH_NAME}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed for branch ${env.BRANCH_NAME}"
        }
    }
}
