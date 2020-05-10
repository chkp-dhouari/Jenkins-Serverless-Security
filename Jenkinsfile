pipeline {
      agent any
      
      
     
     stages {
          
         stage('Clone Github repository') {
            
           steps {
              
             checkout scm
           
             }
  
          }
           
      
       
         stage('CloudGuard Proact Code and Compliance Scan') {
       
            
            steps {
              environment {
                  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'CGW', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                     // some block
         
             
                sh 'sudo npm install -g https://artifactory.app.protego.io/cloudguard-serverless-plugin.tgz'
                sh 'cloudguard proact -m template.yaml '
                   
                        }
                     }

               }
           
          }
    } 
}
