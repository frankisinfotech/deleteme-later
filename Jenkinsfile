pipeline {
  agent any

  stages {

    stage ('Build Image') {
      steps {
        sh ''' 
          printenv
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

  }
}
