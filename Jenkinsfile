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
stage('Code Analysis') {           
            steps {
               //put your code scanner 
                echo 'Code Scanning and Analysis'
            }
        }
 
        stage('Robot Testing') {
            steps {
                //put your Testing
                echo 'Robot Testing Start'
            }
            post{
                success{
                    echo "Robot Testing Successfully"
                }
                failure{
                    echo "Robot Testing Failed"
                }
            }
        }
stage('Docker Build'){
     steps{
         script {
                    sh 'docker build -t pavanbolla/autodockerbuild:latest -f ${WORKSPACE}/Dockerfile .'
                                                }
			post{
                success{
                    echo "Build and Push Successfully"
                }
                failure{
                    echo "Build and Push Failed"
                }
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
