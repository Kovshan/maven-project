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
                build job: 'pipeline-as-code/deploy-to-staging-as-code'
            }
        }

        stage('Deploy to production'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message: 'Approve PRODUCTION deployment?'
                }

                build job: 'pipeline-as-code/deploy-to-prod-as-code'
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