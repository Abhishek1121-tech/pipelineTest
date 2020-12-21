pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git(url: 'https://github.com/Abhishek1121-tech/DMSAPP.git', branch: 'master', credentialsId: 'publicGitMine', changelog: true, poll: true)
      }
    }

    stage('Compile') {
      steps {
        sh 'mvn compile'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn package'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }

    stage('Anchore') {
      steps {
        anchore(bailOnFail: true, name: 'docker.io/library/debian:latest', engineRetries: '2', engineurl: 'http://192.168.200.134:8228/v1', engineverify: true, forceAnalyze: true, policyBundleId: 'anchore_cis_1.13.0_base', engineCredentialsId: 'anchoreCredID')
      }
    }

  }
  environment {
    START = 'test'
  }
}