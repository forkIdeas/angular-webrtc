pipeline {
  agent any
  stages {
    stage('Initialize') {
      steps {
        sh 'npm install && npm update'
        sh '''echo $GIT_BRANCH
echo env.BRANCH_NAME
echo $BRANCH_NAME
echo GIT_BRANCH'''
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
    stage('Deploy') {
      steps {
        sh '''cd dist
cp -rf * /var/www/tr-decode/'''
      }
    }
  }
}