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
          dockerImage1 = docker.build registry_flask + ":$BUILD_NUMBER"
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
          dockerImage2 = docker.build registry_mysql + ":$BUILD_NUMBER"
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
   
   
   
   stage('Modify frontend yaml file') {
	  steps {
           sh "sed -i 's\:4\:$BUILD_NUMBER\g' frontend.yaml"
      }
    }
   
   stage('Copy file to k8s control node') {
      steps{
        dir("${env.WORKSPACE}"){
          sh "scp -r frontend.yaml sheriff23823232@34.100.250.244:/home/sheriff23823232"
          }
      }
   }
   
   stage('Deploy App') {
	  steps {
          sh "ssh sheriff23823232@34.100.250.244 'sleep 5;kubectl create -f frontend.yaml'"
      }
    }
  }
}
