pipeline {
    agent {
        docker {
            image 'hashicorp/terraform' // Use the desired Terraform Docker image
            args  '--entrypoint=""'    // Override the default entrypoint if necessary
        }
    }
    environment {
        AWS_ACCESS_KEY_ID     = credentials('knkaccesskey')
        AWS_SECRET_ACCESS_KEY = credentials('knksecretkey')
        GITHUB_TOKEN          = credentials('knkgit')
        AWS_DEFAULT_REGION    = 'eu-west-1'
    }

    stages {
        stage('Terraform Init') {
            steps {
                script {
                    // Run terraform init
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Run terraform plan and save to a file
                    sh 'terraform plan -out=tfplan'
                }
            }
        }
    }

    post {
        always {
            // Archive Terraform plan file
            archiveArtifacts artifacts: 'tfplan', allowEmptyArchive: true
        }

        success {
            echo 'Terraform scripts executed successfully!'
        }

        failure {
            echo 'Terraform scripts failed!'
        }
    }
}

