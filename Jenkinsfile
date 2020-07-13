pipeline {
    agent any

    parameters {
         string(name: 'Tomcat_Stg', defaultValue: '100.25.180.160', description: 'Staging Server')
         string(name: 'Tomcat_Prd', defaultValue: '100.25.31.37', description: 'Production Server')
    }
    triggers{
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
                        bat "winscp **/target/*.war ec2-user@${params.Tomcat_Stg}:/var/lib/tomcat7/webapps -i NVirginiaKeyPair.pem"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp **/target/*.war ec2-user@${params.Tomcat_Prd}:/var/lib/tomcat7/webapps -i NVirginiaKeyPair.pem"
                    }
                }
            }
        }

    }
}