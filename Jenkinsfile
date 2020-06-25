pipeline {
     agent any
     environment {
           cloudguardAccessToken = credentials('token2')
            }
     stages {
          stage('Cloning Github repository') { 
                steps{
             checkout scm
                }
              }
          stage ("Install dependencies"){
                 steps{
                   sh 'apt-get update && apt-get install -y nodejs && apt-get install -y npm'
                   sh 'npm install -g https://artifactory.app.protego.io/cloudguard-serverless-plugin.tgz'
                 }
               }
          stage('CloudGuard Proact Code and Compliance Scan'){
              steps {
                 script {      
                    try { 
                withAWS(credentials: 'awscreds', region: 'us-east-1'){
                   //code scan and posture management analysis using proact SAST
                   sh 'cloudguard proact -m template.yml -t $cloudguardAccessToken'
                        }
                    } catch (Exception e) {
                       echo "Movin' on to da FSP"  
                     }
                   }
                 }
               }
           
           stage('Adding runtime security to the Lambda function and deploy serverless app to AWS'){
              agent {
                 //docker container containing Check Point CloudGuard FSP and aws cli
                docker { image 'dhouari/devsecops'
                         args '--entrypoint=' }
                       }          
               steps {
                 withAWS(credentials: 'awscreds', region: 'us-east-1'){
                    //Applying FSP runtime security
                    sh 'cloudguard fsp -C template.yml --region us-east-1 -t $cloudguardAccessToken'
                    // creating the cloudformation stack template for the serverless app using the SAM template
                    sh 'aws cloudformation package --template template.protected.yml --s3-bucket cicd-cp --output-template output.template.yml
                    //Deploying the serverless app with the FSP using the cloudformation stack.
                    sh 'aws cloudformation deploy --template-file /var/lib/jenkins/workspace/sam-pipe@2/output.template.yml --stack-name cgsapp --capabilities CAPABILITY_IAM --parameter-overrides ServiceCount=1'

                    }
                 }
             }
  
     } 
}
