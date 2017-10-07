pipeline {
  agent any
  stages {
    stage('Initialize') {
      parallel {
        stage('master') {
          when {
            expression { env.BRANCH_NAME == 'master' }
          }
          steps {
            sh 'export TARGET="production"'
            sh 'export ENVIRONMENT="prod"'
            sh 'npm install && npm update'
          }
        }
        stage('develop') {
          when {
            expression { env.BRANCH_NAME == 'develop' }
          }
          steps {
            sh 'export TARGET="development"'
            sh 'export ENVIRONMENT="dev"'
            sh 'npm install && npm update'
          }
        }
      }
    }
    stage('Build') {
      steps {
        sh 'ng build --target=$TARGET --environment=$ENVIRONMENT'
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
    stage('Deployment') {
      parallel {
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
  }
}
