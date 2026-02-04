pipeline {
  agent any
  environment{
    DJANGO_SECRET_KEY = credentials('github-token')
    DB_PASSWORD = credentials('db-password')
  }
  stages {
    stage('Clone App') {
      steps {
        git branch: 'main', url:'https://github.com/Neha874-ctrl/django-notes-app.git'
      }
    }
    stage('Build') {
      steps {
        sh 'docker compose up -d --build'
      }
    }
   stage('Security Scan') {
  steps {
    sh '''
      echo "🔍Scanning Docker images..."
      trivy image neha874/django-notes-app || true
      trivy image neha874/django-nginx || true
    '''
  }
}


  }
  post {
    always {
      emailext(
        to: 'nehasharma9d16@gmail.com',
        subject: "Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """
        Job: ${env.JOB_NAME}
        Build Number: ${env.BUILD_NUMBER}
        Status: ${currentBuild.currentResult}
        URL: ${env.BUILD_URL}
        """
      )
    }
  }
}
