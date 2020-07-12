pipeline {
    agent any

    parameters {
         string(name: 'Tomcat-Stg', defaultValue: '100.25.180.160', description: 'Staging Server')
         string(name: 'Tomcat-Prd', defaultValue: '100.25.31.37', description: 'Production Server')
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
                        bat "scp -i NVirginiaKeyPair.pem **/target/*.war ec2-user@${params.Tomcat-Stg}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i NVirginiaKeyPair.pem **/target/*.war ec2-user@${params.Tomcat-Prd}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }

    }
}