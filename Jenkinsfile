def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger',
    ]


pipeline {
    agent any
    
    tools {
        terraform 'terraform'
    }
    
    environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
    }
    
    stages {
        stage('git checkout') {
            steps {
                echo 'cloning codebase'
                git branch: 'main', credentialsId: '2630b70a-a7d7-4a36-bbbd-41a9f267a6c2', url: 'https://github.com/tangtijohnpaul/airbnb-infra.git'
                sh 'ls'
            }
        }
        
        stage('terraform init') {
            steps {
                sh 'terraform init'
            }
        }
        
         stage('terraform validate') {
            steps {
                sh 'terraform validate'
            }
        }
          
         stage('terraform plan') {
            steps {
                sh 'terraform plan'
            }
        } 
            
                 stage('Checkov scan') {
            steps {
               
                sh 'sudo yum install python3-pip'           // Install the package python3-pip 
                sh 'sudo yum remove python3-requests'      // Remove the package python3-requests already with the AMI
                sh 'sudo pip3 install requests'            // Use pip3 to install the package called requests  
                sh 'sudo pip3 install checkov'             // Use pip3 to install the package called checkov  
                sh 'checkov -d .' 
           //     sh 'checkov -d . --skip-check CKV_AWS_79,CKV2_AWS_41,CKV_AWS_126,CKV_AWS_135CKV_AWS_79,CKV_AWS_8,CKV_AWS_126,CKV_AWS_135,CKV_AWS_79,CKV_AWS_8,CKV2_AWS_41'   // use checkov to scan the terraform code
            }
        }
         stage('terraform apply') {
            steps {
                sh 'terraform apply --auto-approve'
                // sh 'terraform apply --auto-approve'
                // sh 'terraform destroy --auto-approve'
            }
        } 

        stage('terraform destroy') {
            steps {
                sh 'terraform  --auto-approve'
                // sh 'terraform apply --auto-approve'
                // sh 'terraform destroy --auto-approve'
            }
        }         
   }
   
   post { 
        always { 
            echo 'I will always say Hello again!'
            slackSend channel: '#jjtech-empower-batch', color: COLOR_MAP[currentBuild.currentResult], message: "Done by Jp. *${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
   }  
}
