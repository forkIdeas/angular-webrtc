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
        sh 'ng build --target=development --environment=dev'
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
    stage('Staging') {
      when {
        expression { env.BRANCH_NAME == 'develop' }
      }
      steps {
        sh 'cd dist && cp -rf * /var/www/tr-decode/'
      }
    }
    stage('Production') {
      when {
        expression { env.BRANCH_NAME == 'master' }
      }
      steps {
        sh 'cd dist && cp -rf * /var/www/tr-decode/'
      }
    }
  }
}
