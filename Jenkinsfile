pipeline {
  agent any
  environment{
    DJANGO_SECRET_KEY = credentials('github-token')
    DB_PASSWORD = credentials('db-password')
    DEBUG = 'False'
  }
  stages {
    stage('Clone App') {
      steps {
        git branch: 'main', url:'https://github.com/Neha874-ctrl/django-notes-app.git'
      }
    }
    stage('Build') {
      steps {
        sh '''
        export DJANGO_SECRET_KEY=${DJANGO_SECRET_KEY}
        export DB_PASSWORD=${DB_PASSWORD}
        export DEBUG=${DEBUG}
        docker compose up -d --build
        '''
      }
    }
  }
}
