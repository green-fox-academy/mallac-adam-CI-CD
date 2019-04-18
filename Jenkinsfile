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
          docker.withRegistry( 'https://id.docker.com/login/?next=%2Fid%2Foauth%2Fauthorize%2F%3Fclient_id%3D43f17c5f-9ba4-4f13-853d-9d0074e349a7%26next%3D%252F%253Fref%253Dlogin%26nonce%3DeyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiI0M2YxN2M1Zi05YmE0LTRmMTMtODUzZC05ZDAwNzRlMzQ5YTciLCJleHAiOjE1NTU1ODY3ODksImlhdCI6MTU1NTU4NjQ4OSwicmZwIjoiVlhBUnVHY0RoUjdVeGtPR0xqc0txQT09IiwidGFyZ2V0X2xpbmtfdXJpIjoiLz9yZWY9bG9naW4ifQ.Q69x-NBqzxlO2SZrOQ2qs2UHCD9qqWg3BXKlw4sHbEI%26redirect_uri%3Dhttps%253A%252F%252Fhub.docker.com%252Fsso%252Fcallback%26response_type%3Dcode%26scope%3Dopenid%26state%3DeyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiI0M2YxN2M1Zi05YmE0LTRmMTMtODUzZC05ZDAwNzRlMzQ5YTciLCJleHAiOjE1NTU1ODY3ODksImlhdCI6MTU1NTU4NjQ4OSwicmZwIjoiVlhBUnVHY0RoUjdVeGtPR0xqc0txQT09IiwidGFyZ2V0X2xpbmtfdXJpIjoiLz9yZWY9bG9naW4ifQ.Q69x-NBqzxlO2SZrOQ2qs2UHCD9qqWg3BXKlw4sHbEI', registryCredential ) {
            // dockerImage.push()
            sh 'docker build -f "Dockerfile" -t adambhun/multibranch-ci-cd:latest .'
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