pipeline {

    environment {
        dokcerimagename = "midguar/nodeapp"
        dockerImage = ""
        DOCKERHUB_CREDENTIALS = credentials('midguar-dockerhub')
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
            app = docker.build("midguar/nodeapp")
          }
        }
      }
    
        
      stage('Login') {
        steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        }
      }
      
      stage('Push') {
        steps {
        sh 'docker push midguar/nodeapp:latest'
        }
      }
        
      stage('Deploying App Kubernetes') {
        steps {
          script {
            kubernetesDeploy (configs: 'deploymentservice.yml',kubeconfigID: 'kuberconfigtxt')
          }
        }
      }                  
    }
}
