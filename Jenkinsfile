pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
        EC2_IP = 'your-ec2-instance-ip'
        SSH_CREDENTIALS = credentials('ec2-ssh-credentials')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/application.git'
            }
        }
        
        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh '''
                    docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                    docker-compose push
                    '''
                }
            }
        }
        
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-credentials']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} '
                        if [ ! -d "application" ]; then
                            git clone https://github.com/your-username/application.git
                        else
                            cd application && git pull origin main
                        fi
                        cd application
                        docker-compose down
                        docker-compose up -d
                    '
                    """
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
