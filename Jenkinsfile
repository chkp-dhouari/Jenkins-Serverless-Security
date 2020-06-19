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
                withAWS(credentials: 'AWScreds', region: 'us-east-1'){
                   sh 'sudo apt-get update'
                   sh 'curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -'
                   sh 'sudo apt install -y nodejs'
                   sh 'sudo npm i -g npm'
                   sh 'sudo npm install -g https://artifactory.app.protego.io/cloudguard-serverless-plugin.tgz' 
                   sh 'cloudguard proact -i protego.yaml'
                   sh 'aws cloudformation package --template template.yaml --s3-bucket cg-cicd --output-template template-export.yml'
                   sh 'aws cloudformation deploy --template-file template-export.yml --stack-name demo-pipeline-prod-stack --capabilities CAPABILITY_IAM'
                    }
                     

               }
           
          }
    } 
}
