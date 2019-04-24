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
    stage('Deploy to Docker') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy to EB') {
      steps{
        script {
          sh 'python --version'
          sh 'apt-get install python-pip'
          sh 'pip --version'
          sh 'pip install awsebcli --upgrade --user'
          sh 'eb --version'
          sh 'ls'
          sh 'eb init -p docker $BUILD_NUMBER'
          sh 'eb create $BUILD_NUMBER'
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