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
         withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]){ 
          // sh 'aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 765176032689.dkr.ecr.eu-west-1.amazonaws.com'
          sh 'docker login -u AWS 765176032689.dkr.ecr.eu-west-1.amazonaws.com'
          sh 'docker tag merchantapi 765176032689.dkr.ecr.eu-west-1.amazonaws.com/merchantapi:$BUILD_ID'
          sh 'docker push 765176032689.dkr.ecr.eu-west-1.amazonaws.com/merchantapi:$BUILD_ID'
      }
      }
    }

  }
} 


 // stage ('Publish to ECR') {
 //      steps {
 //        //sh 'aws ecr-public get-login-password --region eu-west-2 | docker login --username AWS --password-stdin public.ecr.aws/t7e2c6o4'
 //        //withAWS(credentials: 'sam-jenkins-demo-credentials', region: 'eu-west-2') {
 //         withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
 //          sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/t7e2c6o4' //985729960198.dkr.ecr.eu-west-2.amazonaws.com'
 //          sh 'docker build -t frankdemo .'
 //          sh 'docker tag frankdemo:latest public.ecr.aws/t7e2c6o4/frankdemo:latest'
 //          sh 'docker push public.ecr.aws/t7e2c6o4/frankdemo:latest'
 //         }
 //       }
