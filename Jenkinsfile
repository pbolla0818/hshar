pipeline {
 
    agent {  label  'Master'  }
     environment {
    registry = "pavanbolla/autodockerbuild"
    registryCredential = 'dockerHub'
}
    stages {
        stage('Initial Notification') {
            steps {
                 //put webhook for your notification channel 
                 echo 'Pipeline Start Notification'
            }
        }
stage('Docker Build image') {
      steps{
        script {
        sh  'docker rm -f $(docker ps -a -q -l)'
        sh  'docker build . -t pavanbolla/autodockerbuild'
        sh  'docker run -it -p 82:80 -d pavanbolla/autodockerbuild'
                }
      }
    }
	
stage('Docker Push Image') {
      steps{
        script {
          sh 'docker push pavanbolla/autodockerbuild'
        }
      }
    }
stage('Docker Test') {
agent {  label  'test'  }
      steps{
        script {
          sh  'sudo docker rm -f $(sudo docker ps -a -q -l)'
          sh  'sudo docker pull pavanbolla/autodockerbuild'
          sh  'sudo docker run -it -p 82:80 -d pavanbolla/autodockerbuild'
        }
      }
    }
}
}
