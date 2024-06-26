pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['apply', 'destroy'],
            description: 'Choose whether to apply or destroy the Terraform configuration'
        )
    }

    environment {
        TERRAFORM_DIR = '.' // Updated to the root directory
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out the repository containing the Terraform files
                git url: 'https://github.com/Lidlid123/jenkins.git', branch: 'main'
            }
        }

        stage('Initialize Terraform') {
            steps {
                dir("${env.TERRAFORM_DIR}") {
                    // Ensure backend configuration is initialized
                    sh 'terraform init'
                }
            }
        }

        stage('Plan Terraform') {
            steps {
                dir("${env.TERRAFORM_DIR}") {
                    sh 'terraform plan -out=plan.out'
                }
            }
        }

        stage('Apply or Destroy Terraform') {
            steps {
                dir("${env.TERRAFORM_DIR}") {
                    script {
                        if (params.ACTION == 'apply') {
                            sh 'terraform apply -auto-approve plan.out'
                        } else if (params.ACTION == 'destroy') {
                            sh 'terraform destroy -auto-approve'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace, send notifications, etc.
            cleanWs()
        }
    }
}
