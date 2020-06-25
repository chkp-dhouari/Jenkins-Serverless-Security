pipeline {
      agent any
     stages {
         
          stage('Clone Github repository') { 
           steps {
                 
             sh 'echo whoami'
             checkout scm
             }
           }
           
          stage('CloudGuard Proact Code and Compliance Scan'){
             agent {
              docker { image 'dhouari/devsecops'
                      args '--entrypoint= -v /var/run/docker.sock:/var/run/docker.sock --privileged'
                      }
                    }
           
              steps {
                 script {      
                    try { 
                withAWS(credentials: 'awscreds', region: 'us-east-1'){
                   sh 'cloudguard proact -vm template.yml'
                        }
                    } catch (Exception e) {
    
                   echo "Movin on to FSP"  
                     }
                   }
                 }
               }
           
           stage('adding runtime security with FSP and deploy serverless app'){
              agent {
                docker { image 'dhouari/cloudguard:test'
                         args '--entrypoint= ' }
                    }
           
               steps {
                 withAWS(credentials: 'awscreds', region: 'us-east-1'){
                    sh 'cloudguard fsp -C template.yml --region us-east-1'
                    sh 'aws cloudformation package --template template.protected.yml --s3-bucket cicd-cp --output-template output.template.yml'
                    sh 'aws cloudformation deploy --template-file /var/lib/jenkins/workspace/sam-pipe@2/output.template.yml --stack-name serverlessapp --capabilities CAPABILITY_IAM'

                    }
                 }
             }
    } 
}
