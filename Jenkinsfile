pipeline {
 
    agent = any
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
stage("Build"){
	 steps {
		echo "Docker Build"
		cd $WORKSPACE
		docker build -f Dockerfile -t pavanbolla/autodockerbuild:$BUILD_NUMBER .  ## use your docker hub repo
		docker login -u pavanbolla/autodockerbuild -p $DOCKER_PWD ## replace lerndevops with your docker hub username
		docker push pavanbolla/autodockerbuild:$BUILD_NUMBER
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
	    
stage('Image Scan') {
            steps {
                //Put your image scanning tool 
                echo 'Image Scanning Start'
            }
            post{
                success{
                    echo "Image Scanning Successfully"
                }
                failure{
                    echo "Image Scanning Failed"
                }
            }
        }
stage("Deploy to Production"){
            when {
                branch 'master'
            }
            steps { 
                kubernetesDeploy kubeconfigId: 'kubeconfig-credentials-id', configs: 'YOUR_YAML_PATH/your_k8s_yaml', enableConfigSubstitution: true  // REPLACE kubeconfigId
 
             }
            post{
                success{
                    echo "Successfully deployed to Production"
                }
                failure{
                    echo "Failed deploying to Production"
                }
            }
        }
stage("Deploy to Staging"){
            when {
                branch 'staging'
            }
            steps {
                kubernetesDeploy kubeconfigId: 'kubeconfig-credentials-id', configs: 'YOUR_YAML_PATH/your_k8s_yaml', enableConfigSubstitution: true  // REPLACE kubeconfigId
             }
            post{
                success{
                    echo "Successfully deployed to Staging"
                }
                failure{
                    echo "Failed deploying to Staging"
                }
            }
        }
}
 
    post{
        always{
step([
             //put your Testing
            ])
        }
        success{
            //notification webhook
            echo 'Pipeline Execution Successfully Notification'
}
        failure{
            //notification webhook
            echo 'Pipeline Execution Failed Notification'
}
    }
}
}
