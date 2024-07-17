pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dh_id')
  }
  stages {
    stage('BUILD') {
      steps {
        sh 'docker build -t shereenfarag/dso-lab:test-trivy .'
      }
    }
    stage('LOGIN') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('TRIVY FS SCAN') {
      steps {
        sh "trivy fs . > trivyfs.txt"
      }
    }
  
    stage('PUSH') {
      steps {
        sh 'docker push shereenfarag/dso-lab:test-trivy'
      }
    }
}
  post {
    always {
      sh 'docker logout'
    }
  }
}
