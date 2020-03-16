pipeline {
   
   agent any
 
 environment {
    registry = "dheeraj2004/dockerswe645"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
   
   

   stages {
         stage('Checkout proj') {
        steps {
            git branch: 'master',
                credentialsId: 'git',
                url: 'https://github.com/Dheeraj-Ganapathy/SWE645.git'

            sh "ls -lat"
        }
    }
         stage('Build Code') {
             steps {
                  dir("HW2") {
                        sh "pwd"
                        sh "mvn clean install"
                        }  
                
                 
             }
  }
        stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":new"
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
        
         
  
   
  stage('Deploy Kubernetes') {
             steps {
                 
                 sh("kubectl delete deployments --all")
                 sh("kubectl apply -f deployment.yml -f service.yml")
                 
             }
  }
      }
   }
