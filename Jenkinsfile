pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/shitalkumar58627/eks-terraform.git']])

          }
        }
        stage("set env variabl"){
            steps{
                sh 'export AWS_PROFILE=ilab'
            }
        }

        stage ("terraform init") {
            steps {
                sh ('terraform init') 
            }
        }
        
        stage ( "Get Approval " ) {
            options {
                timeout (time: 1, unit: 'MINUTES')
            }
            
            steps {
                input " Please approve to proceed EKS clster Deployment process" 
            }
            
        }
        
       stage('terraform Action'){
            steps{
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: "ilab-aws",
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    echo "Terraform action is --> ${action}"
                 sh ('terraform ${action} --auto-approve') 
                }
            }
        }
        
    }
}
