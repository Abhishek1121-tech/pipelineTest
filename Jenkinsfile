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
        sh '''mvn test;
echo "# FYI, this base image is built via test-pr.sh (from https://github.com/docker-library/bashbrew/tree/master/Dockerfile)
FROM oisupport/bashbrew:base

RUN set -eux; \\
	apt-get update; \\
	apt-get install -y --no-install-recommends \\
# wget for downloading files (especially in tests, which run in this environment)
		ca-certificates \\
		wget \\
# git for cloning source code
		git \\
# gawk for diff-pr.sh
		gawk \\
# tar -tf in diff-pr.sh
		bzip2 \\
	; \\
	rm -rf /var/lib/apt/lists/*

ENV DIR /usr/src/official-images
ENV BASHBREW_LIBRARY $DIR/library

WORKDIR $DIR
COPY . $DIR" > ${WORKSPACE}/Dockerfile

echo "docker.io/library/debian:latest" ${WORKSPACE}/Dockerfile > anchore_images;'''
      }
    }

    stage('Anchore') {
      steps {
        anchore(bailOnFail: true, name: 'anchore_images', engineRetries: '300', engineurl: 'http://192.168.200.134:8228/v1', engineverify: true, forceAnalyze: true, engineCredentialsId: 'anchoreCredID')
      }
    }

  }
  environment {
    START = 'test'
  }
}