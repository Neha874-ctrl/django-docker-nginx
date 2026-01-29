pipeline {
  agent any
  stages {
    stage('Clone App') {
      steps {
        git branch: 'main', url: 'https://github.com/Neha874-ctrl/django-notes-app.git'
      }
    }
    stage('Build') {
      steps {
        withCredentials([string(credentialsId: 'github-token', variable: 'DJANGO_SECRET_KEY'),
                         string(credentialsId: 'db-password', variable: 'DB_PASSWORD')]) {
          sh 'docker compose up -d --build'
        }
      }
    }
  }
}

