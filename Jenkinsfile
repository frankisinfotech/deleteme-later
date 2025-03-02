pipeline {
  agent any

  stages {

    stage ('Build Image') {
      steps {
        sh ''' 
          docker build -t merchantapi .
        '''
      }
    }
  

    stage ('Private_ECR') {
      steps {
        withAWS(credentials: 'aws-access', region: 'eu-west-2'){
          sh 'aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 765176032689.dkr.ecr.eu-west-1.amazonaws.com'
          sh 'docker tag merchantapi 765176032689.dkr.ecr.eu-west-1.amazonaws.com/merchantapi:$BUILD_ID'
          sh 'docker push 765176032689.dkr.ecr.eu-west-1.amazonaws.com/merchantapi:$BUILD_ID'
      }
      }
    }

  }
} 
