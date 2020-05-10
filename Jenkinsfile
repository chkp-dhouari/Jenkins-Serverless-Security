pipeline {
      agent any
      environment {
          CG_API_KEY= credentials("CG_API_KEY")
        }
     
     stages {
          
         stage('Clone Github repository') {
            
           steps {
              
             checkout scm
           
             }
  
          }
           
      
       
         stage('CloudGuard Proact Code and Compliance Scan') {
       
            
            steps {
                  
                sh 'sudo npm install -g https://artifactory.app.protego.io/cloudguard-serverless-plugin.tgz'
                sh 'cloudguard proact -m template.yaml '
                   
              }
           

    
           
          }
    } 
}
