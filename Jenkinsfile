pipeline {
  environment {
    dockerimagename = "rahmanbadru/react-app"
    dockerImage = ""
    
  }
  agent any
  stages {
    stage('Checkout Source') {
      env.BRANCH_NAME='main'
      steps {
        git 'https://github.com/RahmanBadru/jenkins-deployment.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub creds'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", 
                                         "service.yaml")
        }
      }
    }
  }
}
