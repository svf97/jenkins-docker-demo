pipeline {
  agent { label 'linux' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dh_id')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t shereenfarag/dso-Lab:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push shereenfarag/dso-Lab:latest'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
