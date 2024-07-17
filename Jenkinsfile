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
        sh 'docker build -t shereenfarag/dso-lab:latest .'
      }
    }
    stage('LOGIN') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('OWASP FS SCAN') {
      steps {
        dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
        dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
      }
    }

    stage('TRIVY FS SCAN') {
      steps {
        sh "trivy fs . > trivyfs.txt"
      }
    }
    
    stage('PUSH') {
      steps {
        sh 'docker push shereenfarag/dso-lab:latest'
      }
    }
  
    stage("TRIVY"){
      steps{
        sh "trivy image shereenfarag/dso-lab:latest > trivy.txt"
        }
    }

    stage('DEPLOY'){
      steps{
        sh 'docker run -d --name dso-lab -p 3000:3000 shereenfarag/dso-lab:latest'
        }
    }

  post {
    always {
      sh 'docker logout'
    }
  }
}
