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

    stage('Build') {
      steps {
        sh '/opt/homebrew/bin/docker build -t devops_ws .'
      }
    }

    stage('Despliegue del servidor') {
      steps {
        sh '''
          /opt/homebrew/bin/docker stop devops_ws || true
          /opt/homebrew/bin/docker run -d -p 8090:8090 --name devops devops_ws
        '''
      }
    }
  }
}