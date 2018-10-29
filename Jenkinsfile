pipeline {
    agent any 

    parameters { 
        string(name: 'tomcat_dev', defaultValue: '54.93.249.235', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '18.195.2.221', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages{

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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat 'pscp -scp \/y -i "F:\\Education\\Performance\\Amazon\\ubuntuaws64bit_2_privatkey.ppk" "C:\\Program Files (x86)\\Jenkins\\workspace\\FullyAutomated\\webapp\\target\\webapp.war" ubuntu@%tomcat_dev%:/var/lib/tomcat8/webapps'
                    }
                }
                
                stage ('Deploy to Production'){
                    steps {
                        bat 'pscp -scp \/y -i F:\\Education\\Performance\\Amazon\\ubuntuaws64bit_2_privatkey.ppk "C:\\Program Files (x86)\\Jenkins\\workspace\\FullyAutomated\\webapp\\target\\webapp.war" ubuntu@%tomcat_prod%:/var/lib/tomcat8/webapps'
                    }
                }
            }

        }    
    }
}    