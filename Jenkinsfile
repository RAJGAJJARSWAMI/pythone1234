pipeline {
    agent any
        environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "rajgajjar/new1234"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'dockerhub-pwd'
        dockerImage = ''
    }
    stages {

        stage ('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/RAJGAJJARSWAMI/pythone1234.git']]])
            }
        }
       
        stage ('Build docker image') {
            steps {
                script {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
                //dockerImage = docker.build registry + ":$BUILD_NUMBER"

                }
            }
        }
       
         // Uploading Docker images into Docker Hub
    stage('Upload Image') {
     steps{   
         script {
            withDockerRegistry(credentialsId: 'dockerhub-pwd2', url: ''){
            dockerImage.push()
            }
        }
      }
    }

    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
   
    stage ('K8S Deploy') {
        steps {
          sh 'kubectl apply -f /home/ubuntu/pythone1234/k8s-deployment.yaml -n prod'        
            }
            
        }
    }
  
}
