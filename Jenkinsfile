pipeline {
  agent any

  tools {
    maven 'maven3.9'   // name from Global Tool Config
    jdk     'Jdk'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean compile'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit '**/target/surefire-reports/*.xml'
        }
      }
    }

    stage('Package') {
      when { expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') } }
      steps {
        sh 'mvn package'
      }
      post {
        success {
          archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
      }
    }
  }

  post {
    success {
      echo '✅ Build, tests, and packaging all succeeded!'
    }
    failure {
      echo '❌ Something failed—check the logs above.'
    }
  }
}
