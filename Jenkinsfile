pipeline {
      agent any
      environment {
           
          CLOUDGUARD_KEY = credentials("CG_API_KEY")
      
        }
     stages {
          
         stage('Clone Github repository') {
            post {
                   always {
                      echo "Send notifications for result: ${currentBuild.result}"
                      slackSend channel: 'https://checkpointsoftwareorg.slack.com/archives/C0123T6CV8S', message: 'started'        
                   }
              }
                
    
           steps {
              
             checkout scm
           
             }
  
          }
           
      
       
         stage('CloudGuard Proact Code and Compliance Scan') {
            agent {

              docker { image 'protego/protego-runtime:latest' }

              }
            steps {

                sh "protego proact  --input protego.yml"

                   
              }
           

    
           
          }
    } 
}
