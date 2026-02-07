pipeline {
  agent any
  environment{
    DJANGO_SECRET_KEY = credentials('github-token')
    DB_PASSWORD = credentials('db-password')
    IMAGE_TAG = "${BUILD_NUMBER}"
  }
  stages {
    stage('Clone App') {
      steps {
        git branch: 'main', url:'https://github.com/Neha874-ctrl/django-notes-app.git'
      }
    }
    stage("Build") {
    steps {
        sh '''
          docker compose build
        '''
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
    stage('Cleanup Docker') {
  steps {
    sh '''
      echo "🧹 Cleaning unused Docker resources..."
      docker container prune -f || true
      docker image prune -f || true
      docker volume prune -f || true
    '''
  }
}



  }
 post {
  success {
    withCredentials([string(credentialsId: 'Discord-Webhook', variable: 'DISCORD_URL')]) {
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
    withCredentials([string(credentialsId: 'Discord-Webhook', variable: 'DISCORD_URL')]) {
      sh '''
        curl -H "Content-Type: application/json" \
        -d '{
          "content": "🚨 **BUILD FAILED** ❌\n📦 Job: '${JOB_NAME}'\n🔢 Build: #'${BUILD_NUMBER}'\n🔗 '${BUILD_URL}'"
        }' \
        $DISCORD_URL
      '''
    }
  }
   cleanup {
  cleanWs()
}

}


}
