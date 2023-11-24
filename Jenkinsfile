pipeline {

  environment {
    dockerimagename = "apvazquez/jenkinscicd"
    dockerImage = ""
    tagImage = env.BUILD_ID
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/apvazquez86/nodeapp_test.git'
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
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push(tagImage)
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
