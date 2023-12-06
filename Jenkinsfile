pipeline {

  environment {
    registry_flask = "sheriff23823232/flask"
	registry_mysql = "sheriff23823232/mysql"
    registryCredential = 'dockerhub'
    dockerImage1 = ""
	dockerImage2 = ""
  }

  agent any
  
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/sheriff23823232/Docker-Project.git'
      }
    }

    stage('Build Flask Image') {
      steps{
        script {
          dockerImage1 = docker.build registry_flask
        }
      }
    }

  
    stage('Navigate to mysql directory') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
    }
   
    stage('Build Mysql Image') {
      steps{
        script {
          dockerImage2 = docker.build registry_mysql
        }
      }
    }
      
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage1.push()
			dockerImage2.push()
          }
        }
      }
    }

	stage('Navigate back to workspace directory') {
      steps{
        dir("${env.WORKSPACE}"){
          sh "pwd"
          }
        }
      }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube2")
        }
      }
    }
	    
  }
}
