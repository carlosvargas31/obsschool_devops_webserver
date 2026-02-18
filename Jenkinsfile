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