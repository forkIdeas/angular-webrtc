pipeline {
  agent any
  stages {
    stage('Initialize') {
      steps {
        sh 'npm install && npm update'
      }
    }
    stage('Build') {
      steps {
        sh 'ng build --target=production --environment=prod'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Chrome": {
            sh 'ng test --single-run --browsers ChromeHeadless'
            
          },
          "Firefox": {
            sh 'ng test --single-run --browsers FirefoxHeadless'
            
          }
        )
      }
    }
    stage('Deploy') {
      steps {
        sh '''cd dist
cp -rf * /var/www/tr-decode/'''
      }
    }
  }
}