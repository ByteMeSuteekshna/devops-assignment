pipeline {
    agent any

    environment {
        AWS_IP = '54.210.155.238'
        AZURE_IP = '172.203.241.165'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ByteMeSuteekshna/devops-assignment.git'
            }
        }

        stage('Deploy to AWS') {
            steps {
                sh '''
                    scp -i /home/ubuntu/.ssh/devops-key.pem \
                        -o StrictHostKeyChecking=no \
                        index-aws.html \
                        ubuntu@${AWS_IP}:/var/www/html/index.html
                    ssh -i /home/ubuntu/.ssh/devops-key.pem \
                        -o StrictHostKeyChecking=no \
                        ubuntu@${AWS_IP} "sudo systemctl restart nginx"
                '''
            }
        }

        stage('Deploy to Azure') {
            steps {
                sh '''
                    scp -i /home/ubuntu/.ssh/devops-azure-key.pem \
                        -o StrictHostKeyChecking=no \
                        index-azure.html \
                        azureuser@${AZURE_IP}:/var/www/html/index.html
                    ssh -i /home/ubuntu/.ssh/devops-azure-key.pem \
                        -o StrictHostKeyChecking=no \
                        azureuser@${AZURE_IP} "sudo systemctl restart nginx"
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
