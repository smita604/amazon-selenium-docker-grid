pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Start Selenium Grid') {
      steps {
        sh 'docker-compose down || true'
        sh 'docker-compose up -d'
        sleep 10
      }
    }
    stage('Run Tests') {
      steps {
        sh 'mvn -B test'
      }
    }
    stage('Print Results') {
      steps {
        archiveArtifacts artifacts: 'target/surefire-reports/**', allowEmptyArchive: true
        sh 'echo "=== TESTS FINISHED — show summary ==="'
        sh 'tail -n +1 target/surefire-reports/*.txt || true'
      }
    }
  }
  post {
    always {
      sh 'docker-compose down || true'
    }
  }
}