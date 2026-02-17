pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Calidad y Entorno') {
      parallel {
        stage('Pruebas de SAST') {
          steps {
            echo 'Ejecución de pruebas de SAST'
          }
        }
        stage('Imprimir Env') {
          steps {
            echo "WORKSPACE: ${env.WORKSPACE}"
          }
        }
      }
    }

    stage('Build') {
      steps {
        sh '/opt/homebrew/bin/docker build -t devops_ws .'
      }
    }
  }
}