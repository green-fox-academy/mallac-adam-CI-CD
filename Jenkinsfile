pipeline {
  environment {
    registry = "adambhun/multibranch-ci-cd"
    registryCredential = 'adambdhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Testing') {
      steps {
        sh 'npm init -y'
        sh 'npm install tape'
        sh 'node anagramtest.js'
      }
    }
    stage('Building image') {
      steps{
        script {
          docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            // dockerImage.push()
            sh 'docker build -f "Dockerfile"'
          }
        }
      }
    }
    stage('Cleanup') {
      steps{
        sh 'docker rmi $registry:$BUILD_NUMBER'
        sh 'rm -r node_modules'
        sh 'rm package.json'
      }
    }
  }
}