pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        echo 'Pulling code from GitHub...'
      }
    }
    stage('Build') {
      steps {
        bat 'mvn clean install'
      }
    }
    stage('Test') {
      steps {
        echo 'Running unit tests...'
      }
    }
    stage('Package') {
      steps {
        echo 'Packaging the JAR file...'
      }
    }
  }
}
