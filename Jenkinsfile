pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dh_id')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t shereenfarag/dso-lab:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push shereenfarag/dso-lab:latest'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}