pipeline {
 
    agent any
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
stage('Building image') {
      steps{
        script {
         docker rm -f  $(docker ps -a -q -l)
	 sudo docker build . -t test
        }
      }
    }
	
stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
}
}
