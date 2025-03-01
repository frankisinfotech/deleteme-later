pipeline {
  agent any

  stages {

    stage ('Build Image') {
      steps {
        sh ''' 
          docker build -t merchantapi .
          docker tag merchantapi frankisinfotech/merchantapi:$BUILD_ID
        '''
      }
    }
    stage ('Publish Image') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'docker push frankisinfotech/merchantapi:$BUILD_ID'
        }
      }
    }
    stage ('Pub_ECR') {
      steps {
        
        sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/g0b5g9q2'
            withAWS(credentials: 'aws-access', region: 'eu-west-2') {
              sh 'docker tag merchantapi public.ecr.aws/g0b5g9q2/merchantapi:$BUILD_ID'
              sh 'docker push public.ecr.aws/g0b5g9q2/merchantapi:$BUILD_ID'
            }
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
