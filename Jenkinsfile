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
    sh 'exit 1'

  }
}


  }
 post {
  success {
    withCredentials([string(credentialsId: 'discord-webhook', variable: 'DISCORD_URL')]) {
      sh '''
        curl -H "Content-Type: application/json" \
        -d '{
          "content": "✅ **BUILD SUCCESS** 🎉\n📦 Job: '${JOB_NAME}'\n🔢 Build: #'${BUILD_NUMBER}'\n🔗 '${BUILD_URL}'"
        }' \
        $DISCORD_URL
      '''
      
    }
  }

  failure {
    withCredentials([string(credentialsId: 'discord-webhook', variable: 'DISCORD_URL')]) {
      sh '''
        curl -H "Content-Type: application/json" \
        -d '{
          "content": "🚨 **BUILD FAILED** ❌\n📦 Job: '${JOB_NAME}'\n🔢 Build: #'${BUILD_NUMBER}'\n🔗 '${BUILD_URL}'"
        }' \
        $DISCORD_URL
      '''
    }
  }
}


}
