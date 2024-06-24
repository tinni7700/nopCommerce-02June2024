pipeline {
    agent {label 'nop'}
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('git') {
            steps {
                // This will use to create git pull
                git url: 'https://github.com/tinni7700/nopCommerce-02June2024.git',
                    branch: 'develop'

            }
        }
        stage('build docker image') {
            steps {
                // This will use to create docker image
                sh 'docker image build -t panky7700/nop-test-terraform:latst .'
                sh 'docker image push panky7700/nop-test-terraform:latst'
            }
        }
        stage('infra provisioning') {
            steps {
                // This will use to create infra structure
                sh 'cd infra/terraform && terraform init && terraform workspace select dev && pwd && terraform apply -var-file="dev.tfvars" -var "build_number=${BUILD_ID}" -auto-approve'
            }
        }
        stage('deploy'){
            steps{
                // This will use to deploy code on aks
                sh 'kubectl apply -f infra/k8s'
            }
        }
    }
}