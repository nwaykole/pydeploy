pipeline {
 environment {
  AWS_ACCESS_KEY_ID= credentials('AWS_ACCESS_KEY_ID')
  AWS_SECRET_ACCESS_KEY= credentials('AWS_SECRET_ACCESS_KEY')
 
 }
 agent any
 stages {
 stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nwaykole/pydeploy.git']]])    
                  }
                      }

 // Building Docker images
    stage('Building image') 
  {
      steps{
        script {
          
         sh 'docker build -t flakapp .' 
               }
           }
   }
  
  //Creating AWS ECR registry terraform

 stage('TF Plan') {
       steps {
         script {
           //cd ecr-create
           sh 'cd ecr-create && terraform init'
          sh 'cd ecr-create && terraform plan'
         }
       }
     }

stage('TF Apply') {
      steps {
        script {
          sh 'cd ecr-create && terraform apply --auto-approve'
         
        }
      }
    }
  }
}

