pipeline {

    environment {
        dokcerimagename = "midguar/nodeapp"
        dockerImage = ""
    }

    agent any
    
    stages {
      
      stage('Git Clone') {
        steps{
          git branch: 'main', url: 'https://github.com/Midguar11/nodejs_app_test.git'
        }
      }
        
      stage('Build Image') {             
        steps{
          script {
            app = docker.build dockerimagename
          }
        }
      }
    
        
      stage('Pushing Image') {
        environment {
                registryCredential = 'midguar-dockerhub'   
            }
        steps{
          script {
            docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
                dockerImage.push("latest")
            }

          }  
        }            
      }
        
      stage('Deploying App Kubernetes') {
        steps {
          script {
            kubernetesDeeploy(configs: "deploymnentservice.yml", kubeconfigID: "kubernetes")
          }
        }
      }                  
    }
}
