pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git(url: 'https://github.com/Abhishek1121-tech/DMSAPP.git', branch: 'master', credentialsId: 'publicGitMine', changelog: true, poll: true)
        sh '''cd ${WORKSPACE}/src
pwd'''
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
echo "FROM debian:9.2

LABEL maintainer "opsxcq@strm.sh"

RUN apt-get update && \\
    apt-get upgrade -y && \\
    DEBIAN_FRONTEND=noninteractive apt-get install -y \\
    debconf-utils && \\
    echo mariadb-server mysql-server/root_password password vulnerables | debconf-set-selections && \\
    echo mariadb-server mysql-server/root_password_again password vulnerables | debconf-set-selections && \\
    DEBIAN_FRONTEND=noninteractive apt-get install -y \\
    apache2 \\
    mariadb-server \\
    php \\
    php-mysql \\
    php-pgsql \\
    php-pear \\
    php-gd \\
    && \\
    apt-get clean && \\
    rm -rf /var/lib/apt/lists/*

COPY php.ini /etc/php5/apache2/php.ini
COPY dvwa /var/www/html

COPY config.inc.php /var/www/html/config/

RUN chown www-data:www-data -R /var/www/html && \\
    rm /var/www/html/index.html

RUN service mysql start && \\
    sleep 3 && \\
    mysql -uroot -pvulnerables -e \\"CREATE USER app@localhost IDENTIFIED BY \'vulnerables\';CREATE DATABASE dvwa;GRANT ALL privileges ON dvwa.* TO \'app\'@localhost;\\"

EXPOSE 80

COPY main.sh /
ENTRYPOINT ["/main.sh"]" > ${WORKSPACE}/Dockerfile

echo "docker.io/vulnerables/web-dvwa:latest" ${WORKSPACE}/Dockerfile > anchore_images;'''
      }
    }

    stage('Anchore') {
      steps {
        anchore(name: 'anchore_images', engineRetries: '300', engineurl: 'http://192.168.200.134:8228/v1', engineverify: true, forceAnalyze: true, engineCredentialsId: 'anchoreCredID', bailOnPluginFail: true)
      }
    }

  }
  environment {
    START = 'test'
  }
}