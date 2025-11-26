pipeline {
  agent any

  environment {
    IMAGE_NAME = "cv-juan-candia"
    CONTAINER_NAME = "cv-container"
    PORT = "8080"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/JFCandia/TU_REPO_CV.git'
      }
    }

    stage('Build Docker image') {
      steps {
        bat 'docker build -t %IMAGE_NAME% .'
      }
    }

    stage('Run container') {
      steps {
        bat 'docker rm -f %CONTAINER_NAME% || exit /b 0'
        bat 'docker run -d --name %CONTAINER_NAME% -p %PORT%:80 %IMAGE_NAME%'
      }
    }

    stage('Smoke test') {
      steps {
        bat 'powershell -Command "(Invoke-WebRequest http://localhost:%PORT%).StatusCode"'
      }
    }
  }

  post {
    success {
      echo "CV desplegado en http://localhost:${PORT}"
    }
    failure {
      echo "Falló el pipeline. Revisa los logs."
    }
  }
}