pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building'
        echo 'PRE: SQL DATABASE CREATION'
        sh 'docker stop db || true'
        sh 'docker run --name db -v db_data:/var/lib/mysql --network some-network -e MYSQL_ROOT_PASSWORD=somewordpress -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=wordpress mysql:5.7'
        echo 'PRE: WORDPRESS CREATION'
        sh 'docker build -t wordpress-app .'
        sh 'docker run --rm -d --name temp-wordpress-app -p 8080:80 --network some-network -e WORDPRESS_DB_HOST=db:3306 -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=wordpress -e WORDPRESS_DB_NAME=wordpress'
        echo 'build ready'
      }
    }
    stage('Testing') {
      steps {
        sh 'docker stop temp-wordpress-app || true'
        echo 'DEFINE SOME TESTING'
      }
    }
    stage('Deploy') {
      steps {
        sh 'docker stop wordpress-app || true'
        sh 'docker run --rm -d --name wordpress-app -p80:80 django-app'
        echo 'DEPLOY'
      }
    }
  }
  post {
      always {
        sh 'docker stop temp-wordpress-app || true'
        echo 'POST: ALWAYS'
      }
      success {
        echo 'POST: SUCCESS'
      }
      failure {
        echo 'POST: FAILED'
      }
  }
}
