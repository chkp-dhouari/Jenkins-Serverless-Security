pipeline {
      agent any
     stages {
         
          stage('Clone Github repository') { 
           steps {
             checkout scm
             }
           }
           
          stage('CloudGuard Proact Code and Compliance Scan'){
             agent {
              docker { image 'dhouari/cloudguard:test'
                      args '--entrypoint= -v /var/run/docker.sock:/var/run/docker.sock'
                      }
                    }
           
              steps {
                 script {      
                    try { 
                withAWS(credentials: 'awscreds', region: 'us-east-1'){
                   sh 'cloudguard proact -vm template.yml'
                        }
                    } catch (Exception e) {
    
                   echo "Code Analysis is BLOCK and recommend not using the source code"  
                     }
                   }
                 }
               }
           
           stage('adding runtime security with FSP and deploy serverless app'){
              agent {
                docker { image 'dhouari/cloudguard:test'
                         args '--entrypoint= -v /var/run/docker.sock:/var/run/docker.sock' }
                    }
           
               steps {
                 withAWS(credentials: 'awscreds', region: 'us-east-1'){
                    sh 'cloudguard fsp -C fsp template.yml --region us-east-1'
                    sh 'aws cloudformation package --template template.protected.yml --s3-bucket cicd-cp --output-template output.template.yml'
                    sh 'aws cloudformation deploy --template-path ${context.WORKSPACE}/output.template.yml --stack-name myapp --capabilities CAPABILITY_IAM'

                    }
                 }
             }
    } 
}
