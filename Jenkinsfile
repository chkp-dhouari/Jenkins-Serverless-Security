pipeline {
     agent {

              docker { image 'protego/protego-runtime:latest' }

              }
      environment {
           
          CLOUDGUARD_KEY = credentials("CG_API_KEY")
      
        }
     stages {
          
         stage('Clone Github repository') {
            
           steps {
              
             checkout scm
           
             }
  
          }
           
      
       
         stage('CloudGuard Proact Code and Compliance Scan') {
            
            steps {

                sh 'protego proact -i protego.yml -t protego-config.json'

                   
              }
           

    
           
          }
    } 
}
