pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git(url: 'https://github.com/Abhishek1121-tech/DMSAPP.git', branch: 'master', credentialsId: 'publicGitMine', changelog: true, poll: true)
      }
    }

    stage('teri maaki chut') {
      parallel {
        stage('teri maaki chut') {
          steps {
            sh 'mvn compile'
          }
        }

        stage('maven') {
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