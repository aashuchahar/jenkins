pipeline {
    agent any
    
    stages {
        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }
        
        stage('Deploy to testing server') {
            steps {
                sh '''
                echo "Installing Apache"
                sudo yum install httpd -y
                sudo mkdir -p /var/www/html 
                sudo cp index.html /var/www/html/
                sudo systemctl stop firewalld
                '''
            }
        }
        
        stage('Technical Team Approval') {
            steps {
                input(
                    message: 'Technical Team is Testing Successfully?', 
                    ok: 'Approve Deployment'
                )
            }
        }
        
        stage("Deploy to Production") {
            steps {
                sh '''
                echo "Installing Apache"
                sudo yum install httpd -y
                sudo mkdir -p /var/www/html 
                sudo cp index.html /var/www/html/
                sudo systemctl start httpd 
                sudo systemctl stop firewalld
                '''
            }
        }
        
        stage('Manager Approval') {
            steps {
                input(
                    message: 'Manager Approve Production Release?', 
                    ok: 'Release'
                )
            }
        }
        
        stage('Production release complete') {
            steps {
                echo "Application Successfully Deployed BY Jenkins"
            }
        }
    }
    
    post {  
        success {
            emailext(
                to: 'aashuchahar9@gmail.com',
                subject: 'Production Deployment Successfully Done',
                body: '''
                Hello Team 
                
                Production Deployment done 
                
                All stages worked 
                '''
            )
        }
        failure {
            emailext(
                to: 'aashuchahar9@gmail.com',
                subject: 'Production Deployment failed',
                body: '''
                Hello Team 
                
                Production Deployment failed 
                
                All stages NOT Worked   
                ''' 
            )
        }
    }
}
