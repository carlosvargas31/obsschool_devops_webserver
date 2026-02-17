pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Pruebas de SAST') {
      steps {
        echo 'Ejecución de pruebas de SAST'
      }
    }

    stage('Build') {
      steps {
        sh '/opt/homebrew/bin/docker build -t devops_ws .'
      }
    }
  }
}