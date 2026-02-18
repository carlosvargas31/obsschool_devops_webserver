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
                find ~/.jenkins/tools -name "sonar-scanner" -executable -type f 2>/dev/null | head -1 | xargs -I {} {} \
                  -Dsonar.projectKey=obsschool_devops_webserver \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=${SONAR_HOST_URL} \
                  -Dsonar.login=${SONAR_AUTH_TOKEN}
              '''
            }
            timeout(time: 5, unit: 'MINUTES') {
              waitForQualityGate abortPipeline: false
            }
          }
        }
        stage('Imprimir Env') {
          steps {
            echo "WORKSPACE: ${env.WORKSPACE}"
          }
        }
      }
    }

    stage('Configurar archivo') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'Credentials_DevOps', usernameVariable: 'USER', passwordVariable: 'PASSWORD')]) {
          sh '''
            cat > credentials.ini <<EOF
[credentials]
user=${USER}
password=${PASSWORD}
EOF
            echo "Archivo credentials.ini creado exitosamente"
          '''
        }
      }
    }

    stage('Build') {
      steps {
        sh '/opt/homebrew/bin/docker build -t devops_ws .'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'credentials.ini', allowEmptyArchive: true
    }
  }
}