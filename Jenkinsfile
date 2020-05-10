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
              
                withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    
         
             
                sh 'sudo npm install -g https://artifactory.app.protego.io/cloudguard-serverless-plugin.tgz'
                sh 'cloudguard proact -m template.yaml '
                   
                        }
                     

               }
           
          }
    } 
}
