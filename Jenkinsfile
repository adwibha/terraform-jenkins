pipeline {
    agent any

    environment {
        // No need to hard-code, use Jenkins credentials ID
        AWS_ACCESS_KEY_ID = ''
        AWS_SECRET_ACCESS_KEY = ''
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git branch: 'main', url: 'https://github.com/adwibha/terraform-jenkins.git'
            }
        }
        
        stage('Set AWS Credentials') {
            steps {
                // Fetch credentials from Jenkins and set as environment variables
                withCredentials([string(credentialsId: 'aws_access_key_id', variable: 'AWS_ACCESS_KEY_ID'),
                                 string(credentialsId: 'aws_secret_access_key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                    echo "AWS Credentials are set securely."
                }
            }
        }

        stage('Terraform Init') {
            steps {
                // Initialize Terraform with AWS credentials
                sh 'terraform init'
            }
        }
        
        stage('Terraform Plan') {
            steps {
                // Run Terraform plan using the secure AWS credentials
                sh 'terraform plan'
            }
        }
        
        stage('Terraform Apply') {
            steps {
                // Apply Terraform plan using the secure AWS credentials
                sh 'terraform apply -auto-approve'
            }
        }
    }

    post {
        always {
            // Clean up workspace after execution
            cleanWs()
        }
        failure {
            // Notify if the pipeline fails
            echo 'Pipeline failed!'
        }
    }
}
