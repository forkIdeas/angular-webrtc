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
            
          },
          "Opera": {
            sh 'ng test --single-run --browsers Opera'
            
          },
          "Safari": {
            sh 'ng test --single-run --browsers Safari'
            
          }
        )
      }
    }
    stage('Package') {
      steps {
        sh 'ng build'
        sh 'find ./dist -path \'*/.*\' -prune -o -type f -print | zip ./angular-webrtc.zip -@'
      }
    }
  }
}