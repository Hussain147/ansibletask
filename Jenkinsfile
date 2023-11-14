pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        AMAZON_KEY_PAIR = 'linux'
        UBUNTU_KEY_PAIR = 'ubuntu-key'
    }

    stages {
        

        stage('Checkout') {
            steps {
                deleteDir()
                sh 'echo cloning repo'
                sh 'git clone https://github.com/Hussain147/ansibletask.git' 
            }
        }
        
        stage('Terraform Apply') {
            steps {
                script {
                    dir('/var/lib/jenkins/workspace/smaple/ansibletask') {
                    sh 'pwd'
                    sh 'terraform init'
                    sh 'terraform validate'
                    // sh 'terraform destroy -auto-approve'
                    sh 'terraform plan'
                    sh 'terraform apply -auto-approve'
                    }
                }
            }
        }
        
        stage('Ansible Deployment') {
            steps {
                script {
                    ansiblePlaybook becomeUser: 'ec2-user', credentialsId: 'amazonlinux', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/smaple/ansibletask/inventory.yaml', playbook: '/var/lib/jenkins/workspace/smaple/ansibletask/amazon-playbook.yml', vaultTmpPath: ''
                    //   ansiblePlaybook become: true, credentialsId: 'ubuntu', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/smaple/ansibletask/inventory.yaml', playbook: '/var/lib/jenkins/workspace/smaple/ansibletask/ubuntu-playbook.yml', vaultTmpPath: ''
                }
            }
        }
    }
}