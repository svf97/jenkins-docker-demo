pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dh_id')
    APP_NAME = "shereenfarag/dso-lab"
  }
  stages {
    stage('BUILD') {
      steps {
        sh "docker build -t $APP_NAME:test-trivy ."
      }
    }
    stage('LOGIN') {
      steps {
        sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
      }
    }
    stage('TRIVY SCAN') {
      steps {
        script{
          // Run Trivy to scan the Docker image
          def trivyOutput = sh(script: "trivy image $APP_NAME:latest", returnStdout: true).trim()

          // Display Trivy scan results
          println trivyOutput

          // Check if vulnerabilities were found
          if (trivyOutput.contains("Total: 0")) {
              echo "No vulnerabilities found in the Docker image."
          } else {
              echo "Vulnerabilities found in the Docker image."
              // You can take further actions here based on your requirements
              // For example, failing the build if vulnerabilities are found
              // error "Vulnerabilities found in the Docker image."
          }
        }
      }
    }
  
    stage('PUSH') {
      steps {
        sh "docker push $APP_NAME:test-trivy"
      }
    }
    stage('DEPLOY'){
      steps{
        sh 'docker run -d --name dso-lab -p 3000:3000 shereenfarag/dso-lab:latest'
        }
    }
}

  post {
    always {
      sh 'docker logout'
    }
  }
  

}