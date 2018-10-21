pipeline {
    agent any
    
    stages {
            
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
        
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage('Deploy to production'){
            steps {
                timeout(time:5s, unit:'DAYS'){
                    input message: 'Approve PRODUCTION deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }    
}