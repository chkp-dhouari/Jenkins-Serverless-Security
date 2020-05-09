pipeline {
      agent any
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
            agent {

              docker { image 'protego/protego-runtime:latest' }

              }
            steps {

                sh 'protego proact  --input protego.yml'

                   
              }
           

    
           
          }
    } 
}
