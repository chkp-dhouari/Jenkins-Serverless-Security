pipeline {
      agent any
       environment {
           cloudguardAccessToken = credentials('token2')
       }
     stages {
         
          stage('Clone Github repository') { 
           steps {
                 
             sh 'echo whoami'
             checkout scm
             }
           }
           
          stage('CloudGuard Proact Code and Compliance Scan'){
              steps {
                 script {      
                    try { 
                withAWS(credentials: 'awscreds', region: 'us-east-1'){
                   sh 'apt-get update && apt-get install -y nodejs && apt-get install -y npm'
                   sh 'npm install -g https://artifactory.app.protego.io/cloudguard-serverless-plugin.tgz'
                   sh 'cloudguard proact -m template.yml -t $cloudguardAccessToken'
                        }
                    } catch (Exception e) {
    
                   echo "Movin' on to da FSP"  
                     }
                   }
                 }
               }
           
           stage('adding runtime security with FSP and deploy serverless app'){
              agent {
                docker { image 'dhouari/devsecops'
                         args '--entrypoint= ' }
                    }
           
               steps {
                 withAWS(credentials: 'awscreds', region: 'us-east-1'){
                    sh 'cloudguard fsp -C template.yml --region us-east-1 -t $cloudguardAccessToken'
                    sh 'aws cloudformation package --template template.protected.yml --s3-bucket cicd-cp --output-template output.template.yml'
                    sh 'aws cloudformation deploy --template-file /var/lib/jenkins/workspace/sam-pipe@2/output.template.yml --stack-name serverlessapp --capabilities CAPABILITY_IAM --parameter-overrides ServiceCount=1'

                    }
                 }
             }
    } 
}
