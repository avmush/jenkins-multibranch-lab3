pipeline {
  agent any

  environment {
    PORT = '' // Will be set later in script block
  }

  stages {
    stage('Set Port') {
      steps {
        script {
          env.PORT = (env.BRANCH_NAME == 'main') ? '3000' : '3001'
        }
        echo "✅ Selected port: ${env.PORT}"
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
        echo "🔧 Building the app for branch ${env.BRANCH_NAME} on port ${env.PORT}"
        // Example: npm install or mvn build, depending on your app
      }
    }

    stage('Test') {
      steps {
        echo "🧪 Running tests..."
        // Example: npm test or mvn test
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("myapp:${env.BRANCH_NAME}")
        }
        echo "🐳 Built Docker image: myapp:${env.BRANCH_NAME}"
      }
    }

    stage('Deploy') {
      steps {
        echo "🚀 Deploying to port ${env.PORT}..."
        // Example: docker run -d -p ${env.PORT}:${env.PORT} myapp:${env.BRANCH_NAME}
      }
    }
  }
}

