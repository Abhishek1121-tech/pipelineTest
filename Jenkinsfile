pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git(url: 'https://github.com/Abhishek1121-tech/DMSAPP.git', branch: 'master', credentialsId: 'publicGitMine', changelog: true, poll: true)
      }
    }

    stage('Compile') {
      parallel {
        stage('Compile') {
          steps {
            sh 'mvn compile'
          }
        }

        stage('verify') {
          steps {
            echo 'Hi keshav'
          }
        }

      }
    }

  }
  environment {
    START = 'test'
  }
}