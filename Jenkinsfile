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
    withCredentials([string(credentialsId: 'Discord-Webhook', variable: 'DISCORD_URL')]) {
      sh """
      curl -H "Content-Type: application/json" \
      -d '{
        "content": "🚀 **Jenkins Build Notification**\\n📦 Job: ${env.JOB_NAME}\\n🔢 Build: #${env.BUILD_NUMBER}\\n📊 Status: ${currentBuild.currentResult}\\n🔗 ${env.BUILD_URL}"
      }' $DISCORD_URL
      """
    }
  }
}

}
