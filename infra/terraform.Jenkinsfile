pipeline {
    agent any

    parameters {
        string(name: 'env', defaultValue: 'default')
        choice(name: 'region', choices: ['us-east-1', 'us-east-2', 'us-west-1', 'us-west-2', 'eu-central-1', 'eu-west-1', 'eu-west-2', 'eu-west-3', 'eu-north-1'])
        booleanParam(name: 'autoApprove', defaultValue: false)
    }

    stages {
        stage('Plan') {
            steps {
                sh """
                cd infra
                terraform init -no-color
                terraform plan -no-color -out tfplan_out --var-file="secret.tfvars"
                """
            }
        }

        stage('Approval') {
            when { expression { !params.autoApprove } }
            steps {
                input message: 'Do you want to apply the Terraform plan?'
            }
        }

        stage('Apply') {
            steps {
                sh "terraform apply tfplan_out"
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'tfplan_out'
        }
    }
}