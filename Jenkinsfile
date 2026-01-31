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
  }
}
