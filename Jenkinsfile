pipeline {
    agent any

    environment {
        EC2_INSTANCE = 'i-xxxxxxxxxxxxxxxxx'
        SSH_KEY = credentials('ec2-ssh-key') // Jenkins SSH key credential
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Sanketcloud25/ci-cd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def remotePath = '/var/www/your-project'
                    sshagent(['ec2-ssh-key']) {
                        sh """
                            scp -r * ec2-user@${EC2_INSTANCE}:${remotePath}
                            ssh ec2-user@${EC2_INSTANCE} 'sudo systemctl restart nginx'
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
