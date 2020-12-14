pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        sh 'mvn compile'
        echo 'hello'
      }
    }

  }
  environment {
    START = 'test'
  }
}