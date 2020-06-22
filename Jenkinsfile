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
              docker { image 'dhouari/cloudguard' 
                       args '--entrypoint="cloudguard"'}
                   }
           
              steps {
                withAWS(credentials: 'awscreds', region: 'us-east-1'){
                   sh 'proact -m template.yml'
                     }
                  }
             }
          stage('adding runtime security with FSP and deploy serverless app'){
            agent {
              docker { image 'dhouari/cloudguard' 
                       args '--entrypoint="cloudguard"'}
                     }
               steps {
                 withAWS(credentials: 'awscreds', region: 'us-east-1'){
                    sh 'fsp -C fsp template.yml --region us-east-1'
                    sh 'aws cloudformation package --template template.protected.yml --s3-bucket cicd-cp --output-template output.template.yml'
                    sh 'aws cloudformation deploy --template-path ${context.WORKSPACE}/output.template.yml --stack-name myapp --capabilities CAPABILITY_IAM'

                    }
                 }
             }
    } 
}
