pipeline {
    agent any
    environment {
        TF_HOME = "/usr/local/bin"   // Terraform installed path
        TF_WORKSPACE = "default"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }
        stage('Terraform Apply') {
            when {
                branch 'main'  // Only apply on main branch
            }
            steps {
                input message: "Approve Terraform Apply?"
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/terraform.tfstate', allowEmptyArchive: true
        }
    }
}
