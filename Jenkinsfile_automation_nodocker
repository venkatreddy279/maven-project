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
                        bat "winscp -i C:\\MISC\\keypair\\NVirginiaKeyPair.pem **/target/*.war ec2-user@${params.Tomcat_Stg}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i C:\\MISC\\keypair\\NVirginiaKeyPair.pem **/target/*.war ec2-user@${params.Tomcat_Prd}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }

    }
}