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
        sh 'ng build'
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
    stage('Package') {
      steps {
        sh 'ng build'
        sh 'mv dist /var/www/html/angular-webrtc'
      }
    }
  }
}