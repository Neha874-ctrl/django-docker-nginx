pipeline {
  agent any
  stages {
    stage('Clone App') {
      steps {
        git 'https://github.com/Neha874-ctrl/django-notes-app.git'
      }
    }
    stage('Build') {
      steps {
        sh 'docker compose up -d --build'
      }
    }
  }
}
