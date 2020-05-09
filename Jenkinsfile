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

                sh "/sourceguard-cli --src ./"

                   }
              }
           
          stage('Docker image Build and scan prep') {
             
            steps {

              sh 'docker build -t dhouari/sg .'
              sh 'docker save dhouari/sg -o sg.tar'
              
             } 
           }
    
           stage('Docker image scan') {
            
               agent {

                 docker { image 'sourceguard/sourceguard-cli' }
   
                   }
               steps {
                    
                  sh "/sourceguard-cli -img sg.tar/"
               
                 
                  }
               }   
           
           stage('Publish to Docker Hub') {
           
                  steps {
                        
                     withDockerRegistry(["https://registry.hub.docker.com", "docker_hub"]) {
                      sh 'docker push dhouari/sg'
                      
                    }
               }     
          }
    } 
}
