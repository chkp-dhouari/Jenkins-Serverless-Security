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

                sh "protego proact -i protego.yml -t eyJhcGlUb2tlbiI6ICJCUkFiVzBPSDlXNElMTDU4TUxqRHI1Z21uTmc3aGlwZTFHeEd6YTFqIiwgImVuZHBvaW50IjogImh0dHBzOi8vbWV0YWxsaWNhLmFwaS5wcm90ZWdvLmlvIiwgInN0YWdlIjogIm1ldGFsbGljYSJ9n"

                   
              }
           

    
           
          }
    } 
}
