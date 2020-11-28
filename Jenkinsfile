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
          sh  'sudo docker pull pavanbolla/autodockerbuild'
                  }
      }
    }
stage("Deploy to Prod"){
agent {  label  'Master'  }
            when {
                branch 'test'
            }
            steps {
	      sh   'kubectl delete -f custom.yml'
              sh   'kubectl create -f custom.yml'
	      sh   'kubectl expose deployment custom-deployment --type NodePort --name custom'
             }
            post{
                success{
                    echo "Successfully deployed to prod"
                }
                failure{
                    echo "Failed deploying to prod"
                }
            }
        }
}
}
