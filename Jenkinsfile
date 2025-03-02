pipeline {
  agent any

  environment {
        ECR_REPO = "765176032689.dkr.ecr.eu-west-1.amazonaws.com/merchantapi"
        REPOSITORY_TAG = "${ECR_REPO}:${BUILD_ID}"
    }
    
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

    stage("Install kubectl"){
      steps {
           sh """
                curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
                chmod +x ./kubectl
                ./kubectl version --client
            """
            }
        }
    stage ('Deploy to Cluster') {
            steps {
                sh "aws eks update-kubeconfig --region eu-west-1 --name ekscluster"
                sh " envsubst < ${WORKSPACE}/deploy.yaml | ./kubectl apply -f - "
            }
        }

  }
} 
