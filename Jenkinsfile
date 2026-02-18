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
            withSonarQubeEnv('SonarQube') {
              sh '''
                /opt/homebrew/bin/sonar-scanner \
                  -Dsonar.projectKey=obsschool_devops_webserver \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=${SONAR_HOST_URL} \
                  -Dsonar.login=${SONAR_AUTH_TOKEN}
              '''
            }
            timeout(time: 1, unit: 'MINUTES') {
              waitForQualityGate abortPipeline: false
            }
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